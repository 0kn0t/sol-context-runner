# Function: runWithState(struct VMCommand[],struct VMState)

**Contract**: [src/VirtualMachine.sol/contract_VirtualMachine.md]

## Metadata

- **Contract**: VirtualMachine
- **Signature**: `runWithState(struct VMCommand[],struct VMState)`
- **Visibility**: public
- **Source Range**: 1234:314:35

## Implementation

```solidity
/// @notice Executes commands with state introspection capabilities.
///  @dev Returns both the final state snapshot and execution output for debugging/analysis.
function runWithState(VMCommand[] calldata commands, VMState memory initialState) public payable returns (VMState memory, bytes memory) {
    bytes memory out = _run(commands, initialState);
    return (initialState, out);
}
```

## Related Implementations

### _run(struct VMCommand[],struct VMState)

- **Kind**: internal
- **Source**: 1959:5177:35
- **Link**: `src/VirtualMachine.sol:VirtualMachine:_run(struct VMCommand[],struct VMState)`

```solidity
/// @dev Primary execution loop with inline opcode handlers for maximum efficiency.
///  All opcodes are handled within this function to eliminate call overhead and
///  maintain optimal register access patterns.
function _run(VMCommand[] calldata commands, VMState memory vmState) internal returns (bytes memory) {
    uint256 len = commands.length;
    for (uint256 i; i < len; ) {
        VMCommand calldata cmd = commands[i];
        OP op = cmd.op;
        bytes32 data = cmd.data;
        if (op == OP.CALL) {
            Call memory c = CommandPacking.unpackCall(data);
            bytes memory callData = vmState.registers.get(c.srcReg.idx());
            if (callData.length < 36) revert VmErrors.InvalidCallDataLength();
            assembly {
                callData := add(callData, 0x20)
            }
            uint256 value;
            if (c.callType == uint8(CallType.VALUECALL)) {
                bytes memory v = vmState.registers.getStatic(c.valueReg.idx());
                assembly {
                    value := mload(add(v, 32))
                }
            }
            (bool success, bytes memory ret) = _performCall(c.target, CallType(c.callType), callData, value);
            if (!success) {
                assembly {
                    let size := returndatasize()
                    returndatacopy(0x00, 0x00, size)
                    revert(0x00, size)
                }
            }
            if (c.destReg.isDyn()) {
                vmState.registers.setDynamic(c.destReg.idx(), ret);
            } else {
                vmState.registers.set(c.destReg.idx(), ret);
            }
        } else if (op == OP.CALLDATA_BUILD) {
            CallDataBuild memory cdb = CommandPacking.unpackCallDataBuild(data);
            bytes memory built = BlueprintEncoder.encodeFromBlueprint(cdb.selector, cdb.blueprint, vmState.registers);
            vmState.registers.set(cdb.destReg.idx(), built);
        } else if (op == OP.EXPLODE) {
            revert VmErrors.Disallowed();
        } else if (op == OP.DEPOSIT_APPROVED) {
            DepositApproved memory d = CommandPacking.unpackDepositApproved(data);
            uint256 depositedAmount = DepositApprovedLib.depositApproved(d);
            vmState.registers.set(d.destReg.idx(), RegisterHelpers.encUint(depositedAmount));
        } else if (op == OP.CALLDATA_SURGERY) {
            CallDataSurgery memory s = CommandPacking.unpackCallDataSurgery(data);
            SurgeryOps.performSurgery(vmState, s.sourceReg, s.surgeries, s.surgeryCount);
        } else if (op == OP.RETURN) {
            Return memory r = CommandPacking.unpackReturn(data);
            return vmState.registers.get(r.sourceReg.idx());
        } else if (op == OP.ABI_ENCODE) {
            AbiEncode memory ae = CommandPacking.unpackAbiEncode(data);
            bytes memory enc = BlueprintEncoder.encodeData(ae.blueprint, vmState.registers);
            vmState.registers.set(ae.destReg.idx(), enc);
        } else if (op == OP.REMAINING_GAS) {
            RemainingGas memory rg = CommandPacking.unpackRemainingGas(data);
            vmState.registers.set(rg.destReg.idx(), RegisterHelpers.encUint(gasleft()));
        } else if (op == OP.NATIVE_BALANCE) {
            NativeBalance memory nb = CommandPacking.unpackNativeBalance(data);
            address targetAddress = vmState.registers.get(nb.addrReg.idx()).asAddress();
            vmState.registers.set(nb.destReg.idx(), RegisterHelpers.encUint(targetAddress.balance));
        } else if (op == OP.LOG) {
            Log memory log = CommandPacking.unpackLog(data);
            VMLogLib.execute(vmState, log);
        } else {
            revert VmErrors.InvalidOpcode(uint8(op));
        }
        unchecked {
            ++i;
        }
    }
    return "";
}
```

### unpackCall(bytes32)

- **Kind**: internal
- **Source**: 4254:623:23
- **Link**: `src/CommandPacking.sol:CommandPacking:unpackCall(bytes32)`

```solidity
///  @notice Unpacks a CALL command from a 32-byte word.
///  @dev Expects format:
///    Byte 0:      Call type (uint8) - variant/sub-opcode for consistency with LOG
///    Bytes 1–20:  Target address (20 bytes)
///    Byte 21:     Destination register (uint8) with dynamic flag in high bit.
///    Byte 22:     Source register (uint8) with dynamic flag in high bit.
///    Byte 23:     Value register (uint8) with dynamic flag in high bit.
///    Bytes 24–31: Padding (ignored)
///  @param packed The packed 32-byte command.
///  @return callCmd A Call struct with the unpacked fields.
function unpackCall(bytes32 packed) internal pure returns (Call memory callCmd) {
    callCmd.callType = uint8(uint256(packed >> 248));
    callCmd.target = address(uint160(uint256(packed >> 88)));
    callCmd.destReg = uint8(uint256(packed >> 80));
    callCmd.srcReg = uint8(uint256(packed >> 72));
    callCmd.valueReg = uint8(uint256(packed >> 64));
}
```

### get(bytes[],uint256)

- **Kind**: internal
- **Source**: 1469:259:30
- **Link**: `src/RegisterFile.sol:RegisterFile:get(bytes[],uint256)`

```solidity
/// @notice Returns the raw data stored in a register.
///  @dev The void register always returns `ZERO_VALUE`.
function get(bytes[] memory registers, uint256 index) internal pure returns (bytes memory) {
    if (index == VmConstants.VOID_REG) return ZERO_VALUE;
    if (index >= registers.length) revert RegisterIndexOOB();
    return registers[index];
}
```

### idx(uint8)

- **Kind**: internal
- **Source**: 259:100:31
- **Link**: `src/RegisterHelpers.sol:RegisterHelpers:idx(uint8)`

```solidity
/// @dev Extract register index (lower 7 bits)
function idx(uint8 r) internal pure returns (uint8) {
    return r & VmConstants.IDX_MASK;
}
```

### getStatic(bytes[],uint256)

- **Kind**: internal
- **Source**: 1877:547:30
- **Link**: `src/RegisterFile.sol:RegisterFile:getStatic(bytes[],uint256)`

```solidity
/// @notice Returns the raw data stored in a register and checks its 32 bytes.
///  @dev The void register always returns `ZERO_VALUE`.
function getStatic(bytes[] memory registers, uint256 index) internal pure returns (bytes memory out) {
    if (index == VmConstants.VOID_REG) return ZERO_VALUE;
    if (index >= registers.length) revert RegisterIndexOOB();
    uint256 len;
    assembly {
        let slot := add(add(registers, 0x20), shl(5, index))
        out := mload(slot)
        len := mload(out)
    }
    if (len != 32) revert InvalidStaticData();
}
```

### _performCall(address,enum CallType,bytes,uint256)

- **Kind**: internal
- **Source**: 7480:580:35
- **Link**: `src/VirtualMachine.sol:VirtualMachine:_performCall(address,enum CallType,bytes,uint256)`

```solidity
/// @dev Low-level call dispatcher supporting all EVM call semantics.
///  Centralizes call logic to ensure consistent error handling and gas management.
function _performCall(address target, CallType t, bytes memory data, uint256 value) internal returns (bool ok, bytes memory ret) {
    if (t == CallType.DELEGATECALL) revert VmErrors.Disallowed();
    if (t == CallType.CALL) return target.call(data);
    if (t == CallType.STATICCALL) return target.staticcall(data);
    return target.call{value: value}(data);
}
```

### isDyn(uint8)

- **Kind**: internal
- **Source**: 435:108:31
- **Link**: `src/RegisterHelpers.sol:RegisterHelpers:isDyn(uint8)`

```solidity
/// @dev Check if register uses dynamic allocation (high bit set)
function isDyn(uint8 r) internal pure returns (bool) {
    return (r & VmConstants.DYN_MASK) != 0;
}
```

### setDynamic(bytes[],uint256,bytes)

- **Kind**: internal
- **Source**: 3375:1077:30
- **Link**: `src/RegisterFile.sol:RegisterFile:setDynamic(bytes[],uint256,bytes)`

```solidity
/// @notice Stores `data` into `register[index]`, dropping the leading
///          0x20 pointer word expected in ABI‑encoded structures. Re-tags the data as bytes by also
///          rewriting the data length as the raw-bytes length (bytes and strings are unchanged).
///          (e.g. `abi.encode(string)`).
///  @dev This function does not work for dynamic tuples and will generate garbage in that case.
function setDynamic(bytes[] memory registers, uint256 index, bytes memory data) internal pure {
    if (index == VmConstants.VOID_REG) return;
    if (index >= registers.length) revert RegisterIndexOOB();
    if (data.length < 0x40) {
        revert InvalidDynamicData();
    }
    uint256 abiOffset;
    assembly {
        abiOffset := mload(add(data, 0x20))
    }
    if (abiOffset != 0x20) {
        revert InvalidDynamicData();
    }
    bytes memory view_;
    assembly {
        view_ := add(data, 0x20)
        mstore(view_, sub(mload(data), 0x20))
    }
    registers[index] = view_;
}
```

### set(bytes[],uint256,bytes)

- **Kind**: internal
- **Source**: 2698:244:30
- **Link**: `src/RegisterFile.sol:RegisterFile:set(bytes[],uint256,bytes)`

```solidity
/// @notice Stores raw `data` into `register[index]`.
function set(bytes[] memory registers, uint256 index, bytes memory data) internal pure {
    if (index == VmConstants.VOID_REG) return;
    if (index >= registers.length) revert RegisterIndexOOB();
    registers[index] = data;
}
```

### unpackCallDataBuild(bytes32)

- **Kind**: internal
- **Source**: 5383:741:23
- **Link**: `src/CommandPacking.sol:CommandPacking:unpackCallDataBuild(bytes32)`

```solidity
///  @notice Unpacks a CALLDATA_BUILD command from a 32-byte word.
///  @dev Expects format:
///    Bytes 0–3:   Function selector (bytes4)
///    Byte 4:      Destination register (uint8) with dynamic flag in high bit
///    Byte 5:      Blueprint length (uint8)
///    Bytes 6–27:  Blueprint (up to 22 bytes)
///    Bytes 28-31: Padding (ignored)
///  @param packed The packed 32-byte command.
///  @return cdb A CallDataBuild struct with the unpacked fields.
function unpackCallDataBuild(bytes32 packed) internal pure returns (CallDataBuild memory cdb) {
    cdb.selector = bytes4(uint32(uint256(packed >> 224)));
    cdb.destReg = uint8(uint256(packed >> 216));
    uint8 blueprintLength = uint8(uint256(packed >> 208));
    cdb.blueprint = new bytes(blueprintLength);
    for (uint256 i = 0; i < blueprintLength; ) {
        unchecked {
            cdb.blueprint[i] = bytes1(uint8(uint256(packed >> ((25 - i) * 8))));
            ++i;
        }
    }
}
```

### encodeFromBlueprint(bytes4,bytes,bytes[])

- **Kind**: internal
- **Source**: 26673:1950:22
- **Link**: `src/BlueprintEncoder.sol:BlueprintEncoder:encodeFromBlueprint(bytes4,bytes,bytes[])`

```solidity
///  @notice Encodes a full ABI payload, prepending a 4-byte function selector.
///  @param selector The function selector.
///  @param blueprint The byte-level layout description.
///  @param registers Caller-supplied data referenced by the blueprint.
///  @return out A (4 + N)-byte ABI blob suitable for a low-level `call`.
function encodeFromBlueprint(bytes4 selector, bytes calldata blueprint, bytes[] memory registers) public pure returns (bytes memory out) {
    if (blueprint.length == 0) {
        out = _allocWithLength(4);
        assembly {
            mstore(add(out, 0x40), selector)
        }
        return out;
    }
    if (_isFlat(blueprint)) {
        (uint256 headB, uint256 tailB) = _measureFlat(blueprint, registers);
        return _encodeFlat(selector, blueprint, registers, headB, tailB);
    }
    uint256[10] memory tmpHeads;
    (uint256 total, uint256 headBytes, uint256 dynCnt) = _measure(blueprint, registers, tmpHeads);
    uint256[] memory dynHead = new uint256[](dynCnt);
    for (uint256 j; j < dynCnt; ) {
        dynHead[j] = tmpHeads[j];
        unchecked {
            ++j;
        }
    }
    uint256 payloadLen = 4 + total;
    out = _allocWithLength(payloadLen);
    assembly {
        mstore(add(out, 0x40), selector)
    }
    _encodeCore(blueprint, registers, dynHead, out, 32 + 4, headBytes);
    return out;
}
```

### _allocWithLength(uint256)

- **Kind**: internal
- **Source**: 39906:181:22
- **Link**: `src/BlueprintEncoder.sol:BlueprintEncoder:_allocWithLength(uint256)`

```solidity
/// @dev Allocates `32 + payloadLen` bytes and writes `payloadLen` as the
///       first 32-byte word of the returned buffer's data area.
function _allocWithLength(uint256 payloadLen) private pure returns (bytes memory out) {
    out = new bytes(LEN_WORD + payloadLen);
    _writeUint(out, 0, payloadLen);
}
```

### _writeUint(bytes,uint256,uint256)

- **Kind**: internal
- **Source**: 39459:295:22
- **Link**: `src/BlueprintEncoder.sol:BlueprintEncoder:_writeUint(bytes,uint256,uint256)`

```solidity
/// @dev Writes a `uint` value into the output buffer at a given offset.
function _writeUint(bytes memory buf, uint256 off, uint256 val) private pure {
    if ((off + 32) > buf.length) revert BufferTooSmall(off + 32, buf.length);
    assembly {
        mstore(add(add(buf, 32), off), val)
    }
}
```

### _isFlat(bytes)

- **Kind**: internal
- **Source**: 14753:456:22
- **Link**: `src/BlueprintEncoder.sol:BlueprintEncoder:_isFlat(bytes)`

```solidity
/// @dev Checks if a blueprint has no container tokens. This allows using the
///       more efficient single-pass encoding path.
function _isFlat(bytes calldata bp) private pure returns (bool flat) {
    uint256 len = bp.length;
    for (uint256 i; i < len; ) {
        uint8 t = uint8(bp[i]);
        if ((t == END_DYNAMIC) || ((t >= START_ARRAY_DYNAMIC) && (t <= START_TUPLE_STATIC))) return false;
        unchecked {
            ++i;
        }
    }
    return true;
}
```

### _measureFlat(bytes,bytes[])

- **Kind**: internal
- **Source**: 15274:1234:22
- **Link**: `src/BlueprintEncoder.sol:BlueprintEncoder:_measureFlat(bytes,bytes[])`

```solidity
/// @dev Single-pass measurement for a flat blueprint.
function _measureFlat(bytes calldata bp, bytes[] memory regs) private pure returns (uint256 headBytes, uint256 tailBytes) {
    uint256 len = bp.length;
    uint256 regCnt = regs.length;
    for (uint256 i; i < len; ) {
        uint8 tok = uint8(bp[i]);
        headBytes += 32;
        if (tok.isDyn()) {
            uint8 idx = tok.idx();
            uint256 blobLen = regs.get(idx).length;
            if (blobLen < 32) revert BadDynamicFormat(idx);
            tailBytes += blobLen;
        } else {
            if (tok > 0x7A) revert InvalidToken();
            if (regs.get(tok).length != 32) revert BadStaticFormat(tok);
        }
        unchecked {
            ++i;
        }
    }
}
```

### _encodeFlat(bytes4,bytes,bytes[],uint256,uint256)

- **Kind**: internal
- **Source**: 16600:707:22
- **Link**: `src/BlueprintEncoder.sol:BlueprintEncoder:_encodeFlat(bytes4,bytes,bytes[],uint256,uint256)`

```solidity
/// @dev Single-pass encoder for a flat blueprint, including a function selector.
function _encodeFlat(bytes4 selector, bytes calldata bp, bytes[] memory regs, uint256 headBytes, uint256 tailBytes) private pure returns (bytes memory out) {
    uint256 payloadLen = (4 + headBytes) + tailBytes;
    out = _allocWithLength(payloadLen);
    assembly {
        mstore(add(out, 0x40), selector)
    }
    _encodeFlatCoreInto(out, bp, regs, headBytes, 32 + 4);
    return out;
}
```

### _encodeFlatCoreInto(bytes,bytes,bytes[],uint256,uint256)

- **Kind**: internal
- **Source**: 17393:2094:22
- **Link**: `src/BlueprintEncoder.sol:BlueprintEncoder:_encodeFlatCoreInto(bytes,bytes,bytes[],uint256,uint256)`

```solidity
/// @dev Core logic for flat encoding, writing into a pre-allocated buffer.
function _encodeFlatCoreInto(bytes memory out, bytes calldata bp, bytes[] memory regs, uint256 headBytes, uint256 startOffset) private pure {
    uint256 headPtr = 0;
    uint256 tailPtr = headBytes;
    uint256 regCnt = regs.length;
    uint256 len = bp.length;
    uint256 headBase = startOffset;
    for (uint256 i; i < len; ) {
        uint8 tok = uint8(bp[i]);
        if ((tok & REG_DYNAMIC_MASK) == 0) {
            if (tok > 0x7A) revert InvalidToken();
            uint8 idx = tok;
            if (regs.get(idx).length != 32) revert BadStaticFormat(idx);
            _copyWord(out, headBase + headPtr, regs.get(idx));
            headPtr += 32;
        } else {
            uint8 idx = tok.idx();
            bytes memory reg = regs.get(idx);
            uint256 dataLen = reg.length;
            if (dataLen < 32) revert BadDynamicFormat(idx);
            uint256 absTail = headBase + tailPtr;
            uint256 relPtr = absTail - headBase;
            _writeUint(out, headBase + headPtr, relPtr);
            uint256 written = _copyDyn(out, absTail, reg);
            headPtr += 32;
            tailPtr += written;
        }
        unchecked {
            ++i;
        }
    }
}
```

### _copyWord(bytes,uint256,bytes)

- **Kind**: internal
- **Source**: 37438:350:22
- **Link**: `src/BlueprintEncoder.sol:BlueprintEncoder:_copyWord(bytes,uint256,bytes)`

```solidity
/// @dev Copies a 32-byte word from a `bytes` register to the output buffer.
function _copyWord(bytes memory out, uint256 dst, bytes memory srcReg) private pure {
    if ((dst + 32) > out.length) revert BufferTooSmall(dst + 32, out.length);
    assembly {
        mstore(add(add(out, 32), dst), mload(add(srcReg, 32)))
    }
}
```

### _copyDyn(bytes,uint256,bytes)

- **Kind**: internal
- **Source**: 37856:817:22
- **Link**: `src/BlueprintEncoder.sol:BlueprintEncoder:_copyDyn(bytes,uint256,bytes)`

```solidity
/// @dev Copies a dynamic, length-prefixed register blob.
function _copyDyn(bytes memory out, uint256 dst, bytes memory reg) private pure returns (uint256 written) {
    uint256 blobLen;
    assembly {
        blobLen := mload(reg)
    }
    if ((dst + blobLen) > out.length) revert BufferTooSmall(dst + blobLen, out.length);
    assembly {
        let srcPtr := add(reg, 32)
        let dstPtr := add(add(out, 32), dst)
        for {
            let off := 0
        } lt(off, blobLen) {
            off := add(off, 32)
        } {
            mstore(add(dstPtr, off), mload(add(srcPtr, off)))
        }
    }
    return blobLen;
}
```

### _measure(bytes,bytes[],uint256[10])

- **Kind**: internal
- **Source**: 20490:5603:22
- **Link**: `src/BlueprintEncoder.sol:BlueprintEncoder:_measure(bytes,bytes[],uint256[10])`

```solidity
/// @dev First pass for nested blueprints. Traverses the blueprint to calculate
///       the total size and gather metadata about dynamic containers.
function _measure(bytes calldata blueprint, bytes[] memory registers, uint256[10] memory tmpDynHeads) private pure returns (uint256 totalBytes, uint256 headBytes, uint256 dynHeadCount) {
    uint256[MAX_STACK_DEPTH] memory stack;
    uint256[MAX_STACK_DEPTH] memory elemCount;
    uint256 sp;
    stack[0] = _mkSizing(0, 0, ContainerKind.NONE);
    elemCount[0] = 0;
    uint256 bpLen = blueprint.length;
    uint256 regCount = registers.length;
    for (uint256 i; i < bpLen; ) {
        uint8 tok = uint8(blueprint[i]);
        ContainerKind open = _decodeKind(tok);
        if (open != ContainerKind.NONE) {
            if ((sp + 1) >= MAX_STACK_DEPTH) revert StackOverflow();
            stack[++sp] = _mkSizing(0, 0, open);
            elemCount[sp] = 0;
        } else if (tok == END_DYNAMIC) {
            if (sp == 0) revert StackUnderflow();
            uint256 child = stack[sp];
            uint256 childElemCount = elemCount[sp];
            sp--;
            ContainerKind ck = _szKind(child);
            uint256 childHeadLen = _szHead(child);
            uint256 childTailLen = _szTail(child);
            ContainerKind parent = _szKind(stack[sp]);
            if ((parent == ContainerKind.DYNAMIC_ARRAY) || (parent == ContainerKind.STATIC_ARRAY)) {
                unchecked {
                    elemCount[sp]++;
                }
            }
            bool dynamic = ((ck == ContainerKind.DYNAMIC_TUPLE) || (ck == ContainerKind.DYNAMIC_ARRAY));
            if (dynamic) {
                if (dynHeadCount >= tmpDynHeads.length) revert DynHeadBufOverflow();
                if (ck == ContainerKind.DYNAMIC_ARRAY) {
                    uint256 packed = childHeadLen | (childElemCount << SHIFT_CNT);
                    tmpDynHeads[dynHeadCount] = packed;
                } else {
                    tmpDynHeads[dynHeadCount] = childHeadLen;
                }
                dynHeadCount++;
                stack[sp] = _szAddHead(stack[sp], 32);
                uint256 bytesChild = childHeadLen + childTailLen;
                if (ck == ContainerKind.DYNAMIC_ARRAY) {
                    stack[sp] = _szAddTail(stack[sp], bytesChild + 32);
                } else {
                    stack[sp] = _szAddTail(stack[sp], bytesChild);
                }
            } else {
                stack[sp] = _szAddHead(stack[sp], childHeadLen);
                stack[sp] = _szAddTail(stack[sp], childTailLen);
            }
        } else if (tok <= 0x7A) {
            stack[sp] = _szAddHead(stack[sp], 32);
            ContainerKind here = _szKind(stack[sp]);
            if ((here == ContainerKind.DYNAMIC_ARRAY) || (here == ContainerKind.STATIC_ARRAY)) {
                unchecked {
                    elemCount[sp]++;
                }
            }
        } else if (tok.isDyn()) {
            uint8 idx = tok.idx();
            uint256 blobLen = (idx < regCount) ? registers.get(idx).length : 0;
            stack[sp] = _szAddHead(stack[sp], 32);
            stack[sp] = _szAddTail(stack[sp], blobLen);
            ContainerKind here = _szKind(stack[sp]);
            if ((here == ContainerKind.DYNAMIC_ARRAY) || (here == ContainerKind.STATIC_ARRAY)) {
                unchecked {
                    elemCount[sp]++;
                }
            }
        } else {
            revert InvalidToken();
        }
        unchecked {
            ++i;
        }
    }
    if (sp != 0) revert UnclosedContainer();
    headBytes = _szHead(stack[0]);
    totalBytes = headBytes + _szTail(stack[0]);
}
```

### _mkSizing(uint256,uint256,enum BlueprintEncoder.ContainerKind)

- **Kind**: internal
- **Source**: 10873:205:22
- **Link**: `src/BlueprintEncoder.sol:BlueprintEncoder:_mkSizing(uint256,uint256,enum BlueprintEncoder.ContainerKind)`

```solidity
/// @dev Constructs a packed Sizing Frame.
function _mkSizing(uint256 headLen, uint256 tailLen, ContainerKind ct) private pure returns (uint256 p) {
    assembly {
        p := or(or(headLen, shl(96, tailLen)), shl(192, ct))
    }
}
```

### _decodeKind(uint8)

- **Kind**: internal
- **Source**: 38960:416:22
- **Link**: `src/BlueprintEncoder.sol:BlueprintEncoder:_decodeKind(uint8)`

```solidity
/// @dev Decodes a container token byte into its `ContainerKind`.
function _decodeKind(uint8 tok) private pure returns (ContainerKind kind) {
    if (tok == START_TUPLE_DYNAMIC) return ContainerKind.DYNAMIC_TUPLE;
    if (tok == START_ARRAY_DYNAMIC) return ContainerKind.DYNAMIC_ARRAY;
    if (tok == START_TUPLE_STATIC) return ContainerKind.STATIC_TUPLE;
    if (tok == START_ARRAY_STATIC) return ContainerKind.STATIC_ARRAY;
    return ContainerKind.NONE;
}
```

### _szKind(uint256)

- **Kind**: internal
- **Source**: 11471:119:22
- **Link**: `src/BlueprintEncoder.sol:BlueprintEncoder:_szKind(uint256)`

```solidity
/// @dev Extracts the ContainerKind from a Sizing Frame.
function _szKind(uint256 p) private pure returns (ContainerKind) {
    return ContainerKind(uint8(p >> 192));
}
```

### _szHead(uint256)

- **Kind**: internal
- **Source**: 11143:94:22
- **Link**: `src/BlueprintEncoder.sol:BlueprintEncoder:_szHead(uint256)`

```solidity
/// @dev Extracts the head length from a Sizing Frame.
function _szHead(uint256 p) private pure returns (uint256) {
    return p & MASK_96;
}
```

### _szTail(uint256)

- **Kind**: internal
- **Source**: 11302:102:22
- **Link**: `src/BlueprintEncoder.sol:BlueprintEncoder:_szTail(uint256)`

```solidity
/// @dev Extracts the tail length from a Sizing Frame.
function _szTail(uint256 p) private pure returns (uint256) {
    return (p >> 96) & MASK_96;
}
```

### _szAddHead(uint256,uint256)

- **Kind**: internal
- **Source**: 11655:106:22
- **Link**: `src/BlueprintEncoder.sol:BlueprintEncoder:_szAddHead(uint256,uint256)`

```solidity
/// @dev Increments the head length in a Sizing Frame.
function _szAddHead(uint256 p, uint256 inc) private pure returns (uint256) {
    return p + inc;
}
```

### _szAddTail(uint256,uint256)

- **Kind**: internal
- **Source**: 11826:114:22
- **Link**: `src/BlueprintEncoder.sol:BlueprintEncoder:_szAddTail(uint256,uint256)`

```solidity
/// @dev Increments the tail length in a Sizing Frame.
function _szAddTail(uint256 p, uint256 inc) private pure returns (uint256) {
    return p + (inc << 96);
}
```

### _encodeCore(bytes,bytes[],uint256[],bytes,uint256,uint256)

- **Kind**: internal
- **Source**: 30679:6457:22
- **Link**: `src/BlueprintEncoder.sol:BlueprintEncoder:_encodeCore(bytes,bytes[],uint256[],bytes,uint256,uint256)`

```solidity
///  @dev Core encoding logic for nested blueprints (Pass 2).
///  @param dynHead Pre-computed metadata for dynamic containers (LIFO).
///  @param out The pre-allocated output buffer.
///  @param startOffset Where to begin writing in `out` (32 or 36 bytes).
///  @param headBytes Pre-measured size of the head section.
function _encodeCore(bytes calldata blueprint, bytes[] memory registers, uint256[] memory dynHead, bytes memory out, uint256 startOffset, uint256 headBytes) private pure {
    uint256 headStart = startOffset;
    uint256 tailStart = startOffset + headBytes;
    uint256[MAX_STACK_DEPTH] memory fs;
    fs[0] = _mkFrame(ContainerKind.NONE, headStart, 0, tailStart, 0);
    uint256 sp;
    uint256 dynPop;
    uint256 regCount = registers.length;
    uint256 bpLen = blueprint.length;
    for (uint256 i; i < bpLen; ) {
        uint8 tok = uint8(blueprint[i]);
        uint256 frame = fs[sp];
        if ((tok >= START_ARRAY_DYNAMIC) && (tok <= START_TUPLE_STATIC)) {
            if ((sp + 1) >= MAX_STACK_DEPTH) revert StackOverflow();
            ContainerKind ck = _decodeKind(tok);
            uint256 childHeadStart;
            uint256 childTailStart;
            uint256 childHeadOff;
            if ((ck == ContainerKind.DYNAMIC_TUPLE) || (ck == ContainerKind.DYNAMIC_ARRAY)) {
                uint256 absPtr = _fTailStart(frame) + _fTailOff(frame);
                uint256 relPtr = absPtr - _fHeadStart(frame);
                _writeUint(out, _fHeadStart(frame) + _fHeadOff(frame), relPtr);
                frame = _fBumpHead(frame, 32);
                if (dynPop >= dynHead.length) revert DynHeadBufOverflow();
                uint256 meta = dynHead[(dynHead.length - 1) - (dynPop++)];
                uint256 childHeadLen = meta & MASK_LEN;
                childHeadStart = absPtr;
                if (ck == ContainerKind.DYNAMIC_ARRAY) {
                    uint256 arrLen = meta >> SHIFT_CNT;
                    _writeUint(out, childHeadStart, arrLen);
                    childHeadOff = 32;
                    childTailStart = (childHeadStart + 32) + childHeadLen;
                    frame = _fBumpTail(frame, 32 + childHeadLen);
                } else {
                    childTailStart = childHeadStart + childHeadLen;
                    frame = _fBumpTail(frame, childHeadLen);
                }
            } else {
                childHeadStart = _fHeadStart(frame) + _fHeadOff(frame);
                childTailStart = _fTailStart(frame) + _fTailOff(frame);
            }
            fs[sp] = frame;
            ++sp;
            fs[sp] = _mkFrame(ck, childHeadStart, childHeadOff, childTailStart, 0);
        } else if (tok == END_DYNAMIC) {
            if (sp == 0) revert StackUnderflow();
            uint256 closed = fs[sp--];
            if ((_fKind(closed) == ContainerKind.DYNAMIC_TUPLE) || (_fKind(closed) == ContainerKind.DYNAMIC_ARRAY)) {
                fs[sp] = _fBumpTail(fs[sp], _fTailOff(closed));
            } else {
                fs[sp] = _fBumpHead(fs[sp], _fHeadOff(closed));
                fs[sp] = _fBumpTail(fs[sp], _fTailOff(closed));
            }
        } else if (tok <= 0x7A) {
            uint8 idx = tok;
            if (registers.get(idx).length != 32) revert BadStaticFormat(idx);
            uint256 dst = _fHeadStart(frame) + _fHeadOff(frame);
            _copyWord(out, dst, registers.get(idx));
            fs[sp] = _fBumpHead(frame, 32);
        } else if (tok.isDyn()) {
            uint8 idx = tok.idx();
            if (registers.get(idx).length < 32) revert BadDynamicFormat(idx);
            uint256 absPtr = _fTailStart(frame) + _fTailOff(frame);
            uint256 relPtr = absPtr - _fHeadStart(frame);
            _writeUint(out, _fHeadStart(frame) + _fHeadOff(frame), relPtr);
            uint256 bytesW = _copyDyn(out, absPtr, registers.get(idx));
            fs[sp] = _fBumpHead(frame, 32);
            fs[sp] = _fBumpTail(fs[sp], bytesW);
        } else {
            revert InvalidToken();
        }
        unchecked {
            ++i;
        }
    }
    if (sp != 0) revert UnclosedContainer();
}
```

### _mkFrame(enum BlueprintEncoder.ContainerKind,uint256,uint256,uint256,uint256)

- **Kind**: internal
- **Source**: 12828:377:22
- **Link**: `src/BlueprintEncoder.sol:BlueprintEncoder:_mkFrame(enum BlueprintEncoder.ContainerKind,uint256,uint256,uint256,uint256)`

```solidity
/// @dev Constructs a packed Encoding Frame.
function _mkFrame(ContainerKind ct, uint256 headStart, uint256 curHeadOff, uint256 tailStart, uint256 curTailOff) private pure returns (uint256 p) {
    assembly {
        p := or(or(or(or(headStart, shl(48, curHeadOff)), shl(96, tailStart)), shl(144, curTailOff)), shl(192, ct))
    }
}
```

### _fTailOff(uint256)

- **Kind**: internal
- **Source**: 13793:105:22
- **Link**: `src/BlueprintEncoder.sol:BlueprintEncoder:_fTailOff(uint256)`

```solidity
/// @dev Extracts the current relative offset within the tail.
function _fTailOff(uint256 p) private pure returns (uint256) {
    return (p >> 144) & MASK_48;
}
```

### _fTailStart(uint256)

- **Kind**: internal
- **Source**: 13614:106:22
- **Link**: `src/BlueprintEncoder.sol:BlueprintEncoder:_fTailStart(uint256)`

```solidity
/// @dev Extracts the absolute start offset of the tail.
function _fTailStart(uint256 p) private pure returns (uint256) {
    return (p >> 96) & MASK_48;
}
```

### _fHeadStart(uint256)

- **Kind**: internal
- **Source**: 13272:98:22
- **Link**: `src/BlueprintEncoder.sol:BlueprintEncoder:_fHeadStart(uint256)`

```solidity
/// @dev Extracts the absolute start offset of the head.
function _fHeadStart(uint256 p) private pure returns (uint256) {
    return p & MASK_48;
}
```

### _fHeadOff(uint256)

- **Kind**: internal
- **Source**: 13443:104:22
- **Link**: `src/BlueprintEncoder.sol:BlueprintEncoder:_fHeadOff(uint256)`

```solidity
/// @dev Extracts the current relative offset within the head.
function _fHeadOff(uint256 p) private pure returns (uint256) {
    return (p >> 48) & MASK_48;
}
```

### _fBumpHead(uint256,uint256)

- **Kind**: internal
- **Source**: 14113:114:22
- **Link**: `src/BlueprintEncoder.sol:BlueprintEncoder:_fBumpHead(uint256,uint256)`

```solidity
/// @dev Bumps the current head offset.
function _fBumpHead(uint256 p, uint256 inc) private pure returns (uint256) {
    return p + (inc << 48);
}
```

### _fBumpTail(uint256,uint256)

- **Kind**: internal
- **Source**: 14277:115:22
- **Link**: `src/BlueprintEncoder.sol:BlueprintEncoder:_fBumpTail(uint256,uint256)`

```solidity
/// @dev Bumps the current tail offset.
function _fBumpTail(uint256 p, uint256 inc) private pure returns (uint256) {
    return p + (inc << 144);
}
```

### _fKind(uint256)

- **Kind**: internal
- **Source**: 13945:118:22
- **Link**: `src/BlueprintEncoder.sol:BlueprintEncoder:_fKind(uint256)`

```solidity
/// @dev Extracts the ContainerKind.
function _fKind(uint256 p) private pure returns (ContainerKind) {
    return ContainerKind(uint8(p >> 192));
}
```

### unpackDepositApproved(bytes32)

- **Kind**: internal
- **Source**: 10734:931:23
- **Link**: `src/CommandPacking.sol:CommandPacking:unpackDepositApproved(bytes32)`

```solidity
///  @notice Unpacks a DEPOSIT_APPROVED command from a 32-byte word.
///  @dev Expects format:
///    Bytes 0-19:  ERC20 token address (20 bytes)
///    Byte 20:     Destination register index (uint8)
///    Bytes 21-31: Unused (ignored)
///  @param packed The packed 32-byte word.
///  @return deposit The unpacked DepositApproved struct.
function unpackDepositApproved(bytes32 packed) internal pure returns (DepositApproved memory deposit) {
    assembly {
        deposit := mload(0x40)
        mstore(0x40, add(deposit, 64))
        let token := shr(96, packed)
        token := and(token, 0x000000000000000000000000ffffffffffffffffffffffffffffffffffffffff)
        mstore(deposit, token)
        let reg := and(shr(88, packed), 0xFF)
        mstore(add(deposit, 32), reg)
    }
}
```

### depositApproved(struct DepositApproved)

- **Kind**: internal
- **Source**: 565:1493:25
- **Link**: `src/DepositApproved.sol:DepositApprovedLib:depositApproved(struct DepositApproved)`

```solidity
/// @notice Executes the deposit approved operation, which deposits ERC20 tokens from an address that previously approved tokens
///  @param depositCmd The DEPOSIT_APPROVED command parameters
function depositApproved(DepositApproved memory depositCmd) internal returns (uint256) {
    IERC20 token = IERC20(depositCmd.token);
    uint256 approvedAmount;
    uint256 userBalance;
    assembly {
        let ptr := mload(0x40)
        mstore(ptr, 0xdd62ed3e00000000000000000000000000000000000000000000000000000000)
        mstore(add(ptr, 0x04), caller())
        mstore(add(ptr, 0x24), address())
        let success := staticcall(gas(), token, ptr, 0x44, ptr, 0x20)
        if and(success, eq(returndatasize(), 0x20)) {
            approvedAmount := mload(ptr)
        }
        mstore(ptr, 0x70a0823100000000000000000000000000000000000000000000000000000000)
        mstore(add(ptr, 0x04), caller())
        success := staticcall(gas(), token, ptr, 0x24, ptr, 0x20)
        if success {
            userBalance := mload(ptr)
        }
    }
    if ((approvedAmount == 0) || (userBalance == 0)) return 0;
    uint256 transferAmount = (approvedAmount < userBalance) ? approvedAmount : userBalance;
    bool transferred = token.transferFrom(msg.sender, address(this), transferAmount);
    if (!transferred) {
        revert VmErrors.TokenTransferFailed();
    }
    return transferAmount;
}
```

### encUint(uint256)

- **Kind**: internal
- **Source**: 615:164:31
- **Link**: `src/RegisterHelpers.sol:RegisterHelpers:encUint(uint256)`

```solidity
/// @dev Encode uint256 as 32-byte array for register storage
function encUint(uint256 x) internal pure returns (bytes memory b) {
    b = new bytes(32);
    assembly {
        mstore(add(b, 32), x)
    }
}
```

### unpackCallDataSurgery(bytes32)

- **Kind**: internal
- **Source**: 14164:1893:23
- **Link**: `src/CommandPacking.sol:CommandPacking:unpackCallDataSurgery(bytes32)`

```solidity
///  @notice Unpacks a CALLDATA_SURGERY command from a 32-byte word.
///  @dev Expects format:
///    Byte 0:      Source register (uint8)
///    Byte 1:      Surgery count (uint8)
///    Bytes 2-25:  Surgery descriptors (up to 6 × 4 bytes)
///    Bytes 26-31: Padding (ignored)
///  @param packed The packed 32-byte command.
///  @return surgery The unpacked CallDataSurgery struct.
function unpackCallDataSurgery(bytes32 packed) internal pure returns (CallDataSurgery memory surgery) {
    surgery.sourceReg = uint8(uint256(packed >> 248));
    surgery.surgeryCount = uint8(uint256(packed >> 240));
    if (surgery.surgeryCount > 6) {
        revert VmErrors.TooManySurgeries();
    }
    for (uint256 i = 0; i < surgery.surgeryCount; ) {
        unchecked {
            uint256 bytePos = 2 + (i * 4);
            uint256 bitPos = 248 - (bytePos * 8);
            uint16 highByte = uint16(uint256(packed >> (bitPos)) & 0xFF);
            uint16 lowByte = uint16(uint256(packed >> (bitPos - 8)) & 0xFF);
            surgery.surgeries[i].offset = (highByte << 8) | lowByte;
            surgery.surgeries[i].length = uint8(uint256(packed >> (bitPos - 16)));
            surgery.surgeries[i].replacementReg = uint8(uint256(packed >> (bitPos - 24)));
            ++i;
        }
    }
    for (uint256 i = surgery.surgeryCount; i < 6; ) {
        surgery.surgeries[i].offset = 0;
        surgery.surgeries[i].length = 0;
        surgery.surgeries[i].replacementReg = 0;
        unchecked {
            ++i;
        }
    }
    return surgery;
}
```

### performSurgery(struct VMState,uint8,struct SurgeryDescriptor[6],uint8)

- **Kind**: internal
- **Source**: 784:2548:33
- **Link**: `src/SurgeryOPS.sol:SurgeryOps:performSurgery(struct VMState,uint8,struct SurgeryDescriptor[6],uint8)`

```solidity
/// @notice Performs surgery operations on calldata, replacing specific byte ranges with new content in place
///  @param vmState The current VM state, containing register data (in-memory)
///  @param sourceReg Register index containing the template calldata (which will be modified in place)
///  @param surgeries Array of surgery descriptors defining the replacements to perform
///  @param surgeryCount Number of valid surgery operations in the array
function performSurgery(VMState memory vmState, uint8 sourceReg, SurgeryDescriptor[6] memory surgeries, uint8 surgeryCount) internal pure {
    uint8 srcRegIndex = sourceReg.idx();
    bytes memory sourceData = vmState.registers[srcRegIndex];
    uint256 sourceLength = sourceData.length;
    for (uint256 i = 0; i < surgeryCount; ) {
        SurgeryDescriptor memory desc = surgeries[i];
        uint8 replacementRegIndex = desc.replacementReg.idx();
        bytes memory replacement = vmState.registers[replacementRegIndex];
        uint256 replacementLength = replacement.length;
        uint256 surgeryLength = desc.length;
        uint256 offset = desc.offset;
        if ((offset + surgeryLength) > sourceLength) {
            revert VmErrors.OutOfBounds();
        }
        if (replacementLength > surgeryLength) {
            revert VmErrors.ReplacementTooLarge();
        }
        assembly {
            let targetPtr := add(add(sourceData, 0x20), offset)
            let replacementPtr := add(replacement, 0x20)
            let padLength := sub(surgeryLength, replacementLength)
            let targetReplacementPtr := add(targetPtr, padLength)
            for {} lt(targetPtr, targetReplacementPtr) {
                targetPtr := add(targetPtr, 1)
            } {
                mstore8(targetPtr, 0)
            }
            for {
                let j := 0
            } lt(j, replacementLength) {
                j := add(j, 1)
            } {
                mstore8(add(targetPtr, j), byte(0, mload(add(replacementPtr, j))))
            }
        }
        unchecked {
            ++i;
        }
    }
}
```

### unpackReturn(bytes32)

- **Kind**: internal
- **Source**: 16993:196:23
- **Link**: `src/CommandPacking.sol:CommandPacking:unpackReturn(bytes32)`

```solidity
///  @notice Unpacks a RETURN command from a 32-byte word.
///  @dev Expects format:
///    Byte 0:      Source register (uint8) containing the data to be returned.
///    Bytes 1-31:  Unused (ignored)
///  @param packed The packed 32-byte command.
///  @return returnCmd A Return struct with the unpacked fields.
function unpackReturn(bytes32 packed) internal pure returns (Return memory returnCmd) {
    returnCmd.sourceReg = uint8(uint256(packed >> 248));
}
```

### unpackAbiEncode(bytes32)

- **Kind**: internal
- **Source**: 19037:640:23
- **Link**: `src/CommandPacking.sol:CommandPacking:unpackAbiEncode(bytes32)`

```solidity
///  @notice Unpacks an ABI_ENCODE command from a 32-byte word.
///  @dev Expects format:
///    Byte 0:      Destination register (uint8) with dynamic flag in high bit.
///    Byte 1:      Blueprint length (uint8)
///    Bytes 2-28:  Blueprint (up to 27 bytes).
///    Bytes 29-31: Padding (ignored)
///  @param packed The packed 32-byte command.
///  @return abiEncode An AbiEncode struct with the unpacked fields.
function unpackAbiEncode(bytes32 packed) internal pure returns (AbiEncode memory abiEncode) {
    abiEncode.destReg = uint8(uint256(packed >> 248));
    uint8 blueprintLength = uint8(uint256(packed >> 240));
    abiEncode.blueprint = new bytes(blueprintLength);
    for (uint256 i = 0; i < blueprintLength; ) {
        unchecked {
            abiEncode.blueprint[i] = bytes1(uint8(uint256(packed >> ((29 - i) * 8))));
            ++i;
        }
    }
}
```

### encodeData(bytes,bytes[])

- **Kind**: internal
- **Source**: 29064:1264:22
- **Link**: `src/BlueprintEncoder.sol:BlueprintEncoder:encodeData(bytes,bytes[])`

```solidity
///  @notice Encodes an ABI data blob *without* a function selector.
///  @param blueprint Layout description.
///  @param registers Caller-supplied registers.
///  @return out ABI-encoded data blob.
function encodeData(bytes calldata blueprint, bytes[] memory registers) public pure returns (bytes memory out) {
    if (blueprint.length == 0) return _allocWithLength(0);
    if (_isFlat(blueprint)) {
        (uint256 headB, uint256 tailB) = _measureFlat(blueprint, registers);
        uint256 payloadLen = headB + tailB;
        out = _allocWithLength(payloadLen);
        _encodeFlatCoreInto(out, blueprint, registers, headB, 32);
        return out;
    }
    uint256[10] memory tmpHeads;
    (uint256 total, uint256 headBytes, uint256 dynCnt) = _measure(blueprint, registers, tmpHeads);
    uint256[] memory dynHead = new uint256[](dynCnt);
    for (uint256 j; j < dynCnt; ) {
        dynHead[j] = tmpHeads[j];
        unchecked {
            ++j;
        }
    }
    out = _allocWithLength(total);
    _encodeCore(blueprint, registers, dynHead, out, 32, headBytes);
    return out;
}
```

### unpackRemainingGas(bytes32)

- **Kind**: internal
- **Source**: 20636:223:23
- **Link**: `src/CommandPacking.sol:CommandPacking:unpackRemainingGas(bytes32)`

```solidity
///  @notice Unpacks a REMAINING_GAS command from a 32-byte word.
///  @dev Expects format:
///    Byte 0:      Destination register (uint8) with dynamic flag in high bit.
///    Bytes 1-31:  Unused (ignored)
///  @param packed The packed 32-byte command.
///  @return remainingGas A RemainingGas struct with the unpacked fields.
function unpackRemainingGas(bytes32 packed) internal pure returns (RemainingGas memory remainingGas) {
    remainingGas.destReg = uint8(uint256(packed >> 248));
}
```

### unpackNativeBalance(bytes32)

- **Kind**: internal
- **Source**: 22226:398:23
- **Link**: `src/CommandPacking.sol:CommandPacking:unpackNativeBalance(bytes32)`

```solidity
///  @notice Unpacks a NATIVE_BALANCE command from a 32-byte word.
///  @dev Expects format:
///    Byte 0:      Address register (uint8) containing the address to get balance of
///    Byte 1:      Destination register (uint8) with dynamic flag in high bit.
///    Bytes 2-31:  Unused (ignored)
///  @param packed The packed 32-byte command.
///  @return nativeBalance A NativeBalance struct with the unpacked fields.
function unpackNativeBalance(bytes32 packed) internal pure returns (NativeBalance memory nativeBalance) {
    nativeBalance.addrReg = uint8(uint256(packed >> 248));
    nativeBalance.destReg = uint8(uint256(packed >> 240));
}
```

### asAddress(bytes)

- **Kind**: internal
- **Source**: 862:207:31
- **Link**: `src/RegisterHelpers.sol:RegisterHelpers:asAddress(bytes)`

```solidity
/// @dev Extract address from bytes (expects abi.encode(address) format)
function asAddress(bytes memory b) internal pure returns (address a) {
    if (b.length != 32) revert VmErrors.InvalidAddressBytes();
    assembly {
        a := mload(add(b, 32))
    }
}
```

### unpackLog(bytes32)

- **Kind**: internal
- **Source**: 24820:343:23
- **Link**: `src/CommandPacking.sol:CommandPacking:unpackLog(bytes32)`

```solidity
///  @notice Unpacks a LOG command from a 32-byte word.
///  @dev Expects format:
///    Byte 0:      Log variant (uint8)
///    Bytes 1-26:  Source registers packed as uint208
///    Bytes 27-31: Padding (ignored)
///  @param packed The packed 32-byte command.
///  @return log A Log struct with the unpacked fields.
function unpackLog(bytes32 packed) internal pure returns (Log memory log) {
    log.variant = uint8(uint256(packed >> 248));
    log.sourceRegs = uint256((uint256(packed >> 40) & ((1 << 208) - 1)));
}
```

### execute(struct VMState,struct Log)

- **Kind**: internal
- **Source**: 1367:1001:34
- **Link**: `src/VMLogLib.sol:VMLogLib:execute(struct VMState,struct Log)`

```solidity
/// @notice Executes the LOG opcode's logic.
///  @param vmState The current VM state (in memory).
///  @param cmd The unpacked Log command parameters.
function execute(VMState memory vmState, Log memory cmd) internal {
    if (cmd.variant > uint8(LogVariant.DYNAMIC)) {
        revert VmErrors.InvalidLogVariant();
    }
    LogVariant variant = LogVariant(cmd.variant);
    if (variant == LogVariant.STATIC_1) {
        executeStatic1(vmState.registers, cmd.sourceRegs);
    } else if (variant == LogVariant.STATIC_2) {
        executeStatic2(vmState.registers, cmd.sourceRegs);
    } else if (variant == LogVariant.STATIC_3) {
        executeStatic3(vmState.registers, cmd.sourceRegs);
    } else if (variant == LogVariant.STATIC_4) {
        executeStatic4(vmState.registers, cmd.sourceRegs);
    } else if (variant == LogVariant.STATIC_5) {
        executeStatic5(vmState.registers, cmd.sourceRegs);
    } else if (variant == LogVariant.DYNAMIC) {
        executeDynamic(vmState.registers, cmd.sourceRegs);
    }
}
```

### executeStatic1(bytes[],uint256)

- **Kind**: internal
- **Source**: 2751:336:34
- **Link**: `src/VMLogLib.sol:VMLogLib:executeStatic1(bytes[],uint256)`

```solidity
/// @notice Executes LOG for 1 static register
function executeStatic1(bytes[] memory registers, uint256 sourceRegs) private {
    uint8 reg1 = extractRegister(sourceRegs, 0);
    bytes memory regData = registers.getStatic(reg1.idx());
    bytes32 data1;
    assembly {
        data1 := mload(add(regData, 0x20))
    }
    emit VMLogStatic1(data1);
}
```

### extractRegister(uint256,uint8)

- **Kind**: internal
- **Source**: 2466:228:34
- **Link**: `src/VMLogLib.sol:VMLogLib:extractRegister(uint256,uint8)`

```solidity
/// @notice Extracts a register index from packed sourceRegs at the given byte position
function extractRegister(uint256 sourceRegs, uint8 position) private pure returns (uint8) {
    return uint8((sourceRegs >> ((25 - position) * 8)));
}
```

### executeStatic2(bytes[],uint256)

- **Kind**: internal
- **Source**: 3145:536:34
- **Link**: `src/VMLogLib.sol:VMLogLib:executeStatic2(bytes[],uint256)`

```solidity
/// @notice Executes LOG for 2 static registers
function executeStatic2(bytes[] memory registers, uint256 sourceRegs) private {
    uint8 reg1 = extractRegister(sourceRegs, 0);
    uint8 reg2 = extractRegister(sourceRegs, 1);
    bytes memory regData1 = registers.getStatic(reg1.idx());
    bytes memory regData2 = registers.getStatic(reg2.idx());
    bytes32 data1;
    bytes32 data2;
    assembly {
        data1 := mload(add(regData1, 0x20))
        data2 := mload(add(regData2, 0x20))
    }
    emit VMLogStatic2(data1, data2);
}
```

### executeStatic3(bytes[],uint256)

- **Kind**: internal
- **Source**: 3739:732:34
- **Link**: `src/VMLogLib.sol:VMLogLib:executeStatic3(bytes[],uint256)`

```solidity
/// @notice Executes LOG for 3 static registers
function executeStatic3(bytes[] memory registers, uint256 sourceRegs) private {
    uint8 reg1 = extractRegister(sourceRegs, 0);
    uint8 reg2 = extractRegister(sourceRegs, 1);
    uint8 reg3 = extractRegister(sourceRegs, 2);
    bytes memory regData1 = registers.getStatic(reg1.idx());
    bytes memory regData2 = registers.getStatic(reg2.idx());
    bytes memory regData3 = registers.getStatic(reg3.idx());
    bytes32 data1;
    bytes32 data2;
    bytes32 data3;
    assembly {
        data1 := mload(add(regData1, 0x20))
        data2 := mload(add(regData2, 0x20))
        data3 := mload(add(regData3, 0x20))
    }
    emit VMLogStatic3(data1, data2, data3);
}
```

### executeStatic4(bytes[],uint256)

- **Kind**: internal
- **Source**: 4529:928:34
- **Link**: `src/VMLogLib.sol:VMLogLib:executeStatic4(bytes[],uint256)`

```solidity
/// @notice Executes LOG for 4 static registers
function executeStatic4(bytes[] memory registers, uint256 sourceRegs) private {
    uint8 reg1 = extractRegister(sourceRegs, 0);
    uint8 reg2 = extractRegister(sourceRegs, 1);
    uint8 reg3 = extractRegister(sourceRegs, 2);
    uint8 reg4 = extractRegister(sourceRegs, 3);
    bytes memory regData1 = registers.getStatic(reg1.idx());
    bytes memory regData2 = registers.getStatic(reg2.idx());
    bytes memory regData3 = registers.getStatic(reg3.idx());
    bytes memory regData4 = registers.getStatic(reg4.idx());
    bytes32 data1;
    bytes32 data2;
    bytes32 data3;
    bytes32 data4;
    assembly {
        data1 := mload(add(regData1, 0x20))
        data2 := mload(add(regData2, 0x20))
        data3 := mload(add(regData3, 0x20))
        data4 := mload(add(regData4, 0x20))
    }
    emit VMLogStatic4(data1, data2, data3, data4);
}
```

### executeStatic5(bytes[],uint256)

- **Kind**: internal
- **Source**: 5515:1124:34
- **Link**: `src/VMLogLib.sol:VMLogLib:executeStatic5(bytes[],uint256)`

```solidity
/// @notice Executes LOG for 5 static registers
function executeStatic5(bytes[] memory registers, uint256 sourceRegs) private {
    uint8 reg1 = extractRegister(sourceRegs, 0);
    uint8 reg2 = extractRegister(sourceRegs, 1);
    uint8 reg3 = extractRegister(sourceRegs, 2);
    uint8 reg4 = extractRegister(sourceRegs, 3);
    uint8 reg5 = extractRegister(sourceRegs, 4);
    bytes memory regData1 = registers.getStatic(reg1.idx());
    bytes memory regData2 = registers.getStatic(reg2.idx());
    bytes memory regData3 = registers.getStatic(reg3.idx());
    bytes memory regData4 = registers.getStatic(reg4.idx());
    bytes memory regData5 = registers.getStatic(reg5.idx());
    bytes32 data1;
    bytes32 data2;
    bytes32 data3;
    bytes32 data4;
    bytes32 data5;
    assembly {
        data1 := mload(add(regData1, 0x20))
        data2 := mload(add(regData2, 0x20))
        data3 := mload(add(regData3, 0x20))
        data4 := mload(add(regData4, 0x20))
        data5 := mload(add(regData5, 0x20))
    }
    emit VMLogStatic5(data1, data2, data3, data4, data5);
}
```

### executeDynamic(bytes[],uint256)

- **Kind**: internal
- **Source**: 6691:221:34
- **Link**: `src/VMLogLib.sol:VMLogLib:executeDynamic(bytes[],uint256)`

```solidity
/// @notice Executes LOG for dynamic data
function executeDynamic(bytes[] memory registers, uint256 sourceRegs) private {
    uint8 reg = extractRegister(sourceRegs, 0);
    bytes memory data = registers.get(reg.idx());
    emit VMLogDyn(data);
}
```

## State Variable Reads

- **ZERO_VALUE** (`bytes`)
- **LEN_WORD** (`uint256`)
- **END_DYNAMIC** (`uint8`)
- **START_ARRAY_DYNAMIC** (`uint8`)
- **START_TUPLE_STATIC** (`uint8`)
- **REG_DYNAMIC_MASK** (`uint8`)
- **MAX_STACK_DEPTH** (`uint8`)
- **SHIFT_CNT** (`uint256`)
- **START_TUPLE_DYNAMIC** (`uint8`)
- **START_ARRAY_STATIC** (`uint8`)
- **MASK_96** (`uint256`)
- **MASK_LEN** (`uint256`)
- **MASK_48** (`uint256`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: VirtualMachine.runWithState(struct VMCommand[],struct VMState) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
  └─ [1] ⚙️ FUNCTION: VirtualMachine._run(struct VMCommand[],struct VMState) (NodeID: 1)
      💬 Args: [commands, initialState]
      👁️  Def: internal
    ├─ [2] ⚙️ FUNCTION: CommandPacking.unpackCall(bytes32) (NodeID: 2)
    │   💬 Args: [data]
    │   👁️  Def: internal
    ├─ [2] ⚙️ FUNCTION: RegisterFile.get(bytes[],uint256) (NodeID: 3)
    │   💬 Args: [vmState.registers, c.srcReg.idx()]
    │   👁️  Def: internal
    │ └─ [3] ⚙️ FUNCTION: RegisterHelpers.idx(uint8) (NodeID: 4)
    │     💬 Args: [c.srcReg]
    │     👁️  Def: internal
    ├─ [2] ⚙️ FUNCTION: RegisterFile.getStatic(bytes[],uint256) (NodeID: 5)
    │   💬 Args: [vmState.registers, c.valueReg.idx()]
    │   👁️  Def: internal
    │ └─ [3] ⚙️ FUNCTION: RegisterHelpers.idx(uint8) (NodeID: 6)
    │     💬 Args: [c.valueReg]
    │     👁️  Def: internal
    ├─ [2] ⚙️ FUNCTION: VirtualMachine._performCall(address,enum CallType,bytes,uint256) (NodeID: 7)
    │   💬 Args: [c.target, CallType(c.callType), callData, value]
    │   👁️  Def: internal
    ├─ [2] ⚙️ FUNCTION: RegisterHelpers.isDyn(uint8) (NodeID: 8)
    │   💬 Args: [c.destReg]
    │   👁️  Def: internal
    ├─ [2] ⚙️ FUNCTION: RegisterFile.setDynamic(bytes[],uint256,bytes) (NodeID: 9)
    │   💬 Args: [vmState.registers, c.destReg.idx(), ret]
    │   👁️  Def: internal
    │ └─ [3] ⚙️ FUNCTION: RegisterHelpers.idx(uint8) (NodeID: 10)
    │     💬 Args: [c.destReg]
    │     👁️  Def: internal
    ├─ [2] ⚙️ FUNCTION: RegisterFile.set(bytes[],uint256,bytes) (NodeID: 11)
    │   💬 Args: [vmState.registers, c.destReg.idx(), ret]
    │   👁️  Def: internal
    │ └─ [3] ⚙️ FUNCTION: RegisterHelpers.idx(uint8) (NodeID: 12)
    │     💬 Args: [c.destReg]
    │     👁️  Def: internal
    ├─ [2] ⚙️ FUNCTION: CommandPacking.unpackCallDataBuild(bytes32) (NodeID: 13)
    │   💬 Args: [data]
    │   👁️  Def: internal
    ├─ [2] ⚙️ FUNCTION: BlueprintEncoder.encodeFromBlueprint(bytes4,bytes,bytes[]) (NodeID: 14)
    │   💬 Args: [cdb.selector, cdb.blueprint, vmState.registers]
    │   👁️  Def: public
    │ ├─ [3] ⚙️ FUNCTION: BlueprintEncoder._allocWithLength(uint256) (NodeID: 15)
    │ │   💬 Args: [4]
    │ │   👁️  Def: private
    │ │ └─ [4] ⚙️ FUNCTION: BlueprintEncoder._writeUint(bytes,uint256,uint256) (NodeID: 16)
    │ │     💬 Args: [out, 0, payloadLen]
    │ │     👁️  Def: private
    │ ├─ [3] ⚙️ FUNCTION: BlueprintEncoder._isFlat(bytes) (NodeID: 17)
    │ │   💬 Args: [blueprint]
    │ │   👁️  Def: private
    │ ├─ [3] ⚙️ FUNCTION: BlueprintEncoder._measureFlat(bytes,bytes[]) (NodeID: 18)
    │ │   💬 Args: [blueprint, registers]
    │ │   👁️  Def: private
    │ │ ├─ [4] ⚙️ FUNCTION: RegisterHelpers.isDyn(uint8) (NodeID: 19)
    │ │ │   💬 Args: [tok]
    │ │ │   👁️  Def: internal
    │ │ ├─ [4] ⚙️ FUNCTION: RegisterHelpers.idx(uint8) (NodeID: 20)
    │ │ │   💬 Args: [tok]
    │ │ │   👁️  Def: internal
    │ │ ├─ [4] ⚙️ FUNCTION: RegisterFile.get(bytes[],uint256) (NodeID: 21)
    │ │ │   💬 Args: [regs, idx]
    │ │ │   👁️  Def: internal
    │ │ └─ [4] ⚙️ FUNCTION: RegisterFile.get(bytes[],uint256) (NodeID: 22)
    │ │     💬 Args: [regs, tok]
    │ │     👁️  Def: internal
    │ ├─ [3] ⚙️ FUNCTION: BlueprintEncoder._encodeFlat(bytes4,bytes,bytes[],uint256,uint256) (NodeID: 23)
    │ │   💬 Args: [selector, blueprint, registers, headB, tailB]
    │ │   👁️  Def: private
    │ │ ├─ [4] ⚙️ FUNCTION: BlueprintEncoder._allocWithLength(uint256) (NodeID: 24)
    │ │ │   💬 Args: [payloadLen]
    │ │ │   👁️  Def: private
    │ │ │ └─ [5] ⚙️ FUNCTION: BlueprintEncoder._writeUint(bytes,uint256,uint256) (NodeID: 25)
    │ │ │     💬 Args: [out, 0, payloadLen]
    │ │ │     👁️  Def: private
    │ │ └─ [4] ⚙️ FUNCTION: BlueprintEncoder._encodeFlatCoreInto(bytes,bytes,bytes[],uint256,uint256) (NodeID: 26)
    │ │     💬 Args: [out, bp, regs, headBytes, 32 + 4]
    │ │     👁️  Def: private
    │ │   ├─ [5] ⚙️ FUNCTION: RegisterFile.get(bytes[],uint256) (NodeID: 27)
    │ │   │   💬 Args: [regs, idx]
    │ │   │   👁️  Def: internal
    │ │   ├─ [5] ⚙️ FUNCTION: BlueprintEncoder._copyWord(bytes,uint256,bytes) (NodeID: 28)
    │ │   │   💬 Args: [out, headBase + headPtr, regs.get(idx)]
    │ │   │   👁️  Def: private
    │ │   │ └─ [6] ⚙️ FUNCTION: RegisterFile.get(bytes[],uint256) (NodeID: 29)
    │ │   │     💬 Args: [regs, idx]
    │ │   │     👁️  Def: internal
    │ │   ├─ [5] ⚙️ FUNCTION: RegisterHelpers.idx(uint8) (NodeID: 30)
    │ │   │   💬 Args: [tok]
    │ │   │   👁️  Def: internal
    │ │   ├─ [5] ⚙️ FUNCTION: RegisterFile.get(bytes[],uint256) (NodeID: 31)
    │ │   │   💬 Args: [regs, idx]
    │ │   │   👁️  Def: internal
    │ │   ├─ [5] ⚙️ FUNCTION: BlueprintEncoder._writeUint(bytes,uint256,uint256) (NodeID: 32)
    │ │   │   💬 Args: [out, headBase + headPtr, relPtr]
    │ │   │   👁️  Def: private
    │ │   └─ [5] ⚙️ FUNCTION: BlueprintEncoder._copyDyn(bytes,uint256,bytes) (NodeID: 33)
    │ │       💬 Args: [out, absTail, reg]
    │ │       👁️  Def: private
    │ ├─ [3] ⚙️ FUNCTION: BlueprintEncoder._measure(bytes,bytes[],uint256[10]) (NodeID: 34)
    │ │   💬 Args: [blueprint, registers, tmpHeads]
    │ │   👁️  Def: private
    │ │ ├─ [4] ⚙️ FUNCTION: BlueprintEncoder._mkSizing(uint256,uint256,enum BlueprintEncoder.ContainerKind) (NodeID: 35)
    │ │ │   💬 Args: [0, 0, ContainerKind.NONE]
    │ │ │   👁️  Def: private
    │ │ ├─ [4] ⚙️ FUNCTION: BlueprintEncoder._decodeKind(uint8) (NodeID: 36)
    │ │ │   💬 Args: [tok]
    │ │ │   👁️  Def: private
    │ │ ├─ [4] ⚙️ FUNCTION: BlueprintEncoder._mkSizing(uint256,uint256,enum BlueprintEncoder.ContainerKind) (NodeID: 37)
    │ │ │   💬 Args: [0, 0, open]
    │ │ │   👁️  Def: private
    │ │ ├─ [4] ⚙️ FUNCTION: BlueprintEncoder._szKind(uint256) (NodeID: 38)
    │ │ │   💬 Args: [child]
    │ │ │   👁️  Def: private
    │ │ ├─ [4] ⚙️ FUNCTION: BlueprintEncoder._szHead(uint256) (NodeID: 39)
    │ │ │   💬 Args: [child]
    │ │ │   👁️  Def: private
    │ │ ├─ [4] ⚙️ FUNCTION: BlueprintEncoder._szTail(uint256) (NodeID: 40)
    │ │ │   💬 Args: [child]
    │ │ │   👁️  Def: private
    │ │ ├─ [4] ⚙️ FUNCTION: BlueprintEncoder._szKind(uint256) (NodeID: 41)
    │ │ │   💬 Args: [stack[sp]]
    │ │ │   👁️  Def: private
    │ │ ├─ [4] ⚙️ FUNCTION: BlueprintEncoder._szAddHead(uint256,uint256) (NodeID: 42)
    │ │ │   💬 Args: [stack[sp], 32]
    │ │ │   👁️  Def: private
    │ │ ├─ [4] ⚙️ FUNCTION: BlueprintEncoder._szAddTail(uint256,uint256) (NodeID: 43)
    │ │ │   💬 Args: [stack[sp], bytesChild + 32]
    │ │ │   👁️  Def: private
    │ │ ├─ [4] ⚙️ FUNCTION: BlueprintEncoder._szAddTail(uint256,uint256) (NodeID: 44)
    │ │ │   💬 Args: [stack[sp], bytesChild]
    │ │ │   👁️  Def: private
    │ │ ├─ [4] ⚙️ FUNCTION: BlueprintEncoder._szAddHead(uint256,uint256) (NodeID: 45)
    │ │ │   💬 Args: [stack[sp], childHeadLen]
    │ │ │   👁️  Def: private
    │ │ ├─ [4] ⚙️ FUNCTION: BlueprintEncoder._szAddTail(uint256,uint256) (NodeID: 46)
    │ │ │   💬 Args: [stack[sp], childTailLen]
    │ │ │   👁️  Def: private
    │ │ ├─ [4] ⚙️ FUNCTION: BlueprintEncoder._szAddHead(uint256,uint256) (NodeID: 47)
    │ │ │   💬 Args: [stack[sp], 32]
    │ │ │   👁️  Def: private
    │ │ ├─ [4] ⚙️ FUNCTION: BlueprintEncoder._szKind(uint256) (NodeID: 48)
    │ │ │   💬 Args: [stack[sp]]
    │ │ │   👁️  Def: private
    │ │ ├─ [4] ⚙️ FUNCTION: RegisterHelpers.isDyn(uint8) (NodeID: 49)
    │ │ │   💬 Args: [tok]
    │ │ │   👁️  Def: internal
    │ │ ├─ [4] ⚙️ FUNCTION: RegisterHelpers.idx(uint8) (NodeID: 50)
    │ │ │   💬 Args: [tok]
    │ │ │   👁️  Def: internal
    │ │ ├─ [4] ⚙️ FUNCTION: RegisterFile.get(bytes[],uint256) (NodeID: 51)
    │ │ │   💬 Args: [registers, idx]
    │ │ │   👁️  Def: internal
    │ │ ├─ [4] ⚙️ FUNCTION: BlueprintEncoder._szAddHead(uint256,uint256) (NodeID: 52)
    │ │ │   💬 Args: [stack[sp], 32]
    │ │ │   👁️  Def: private
    │ │ ├─ [4] ⚙️ FUNCTION: BlueprintEncoder._szAddTail(uint256,uint256) (NodeID: 53)
    │ │ │   💬 Args: [stack[sp], blobLen]
    │ │ │   👁️  Def: private
    │ │ ├─ [4] ⚙️ FUNCTION: BlueprintEncoder._szKind(uint256) (NodeID: 54)
    │ │ │   💬 Args: [stack[sp]]
    │ │ │   👁️  Def: private
    │ │ ├─ [4] ⚙️ FUNCTION: BlueprintEncoder._szHead(uint256) (NodeID: 55)
    │ │ │   💬 Args: [stack[0]]
    │ │ │   👁️  Def: private
    │ │ └─ [4] ⚙️ FUNCTION: BlueprintEncoder._szTail(uint256) (NodeID: 56)
    │ │     💬 Args: [stack[0]]
    │ │     👁️  Def: private
    │ ├─ [3] ⚙️ FUNCTION: BlueprintEncoder._allocWithLength(uint256) (NodeID: 57)
    │ │   💬 Args: [payloadLen]
    │ │   👁️  Def: private
    │ │ └─ [4] ⚙️ FUNCTION: BlueprintEncoder._writeUint(bytes,uint256,uint256) (NodeID: 58)
    │ │     💬 Args: [out, 0, payloadLen]
    │ │     👁️  Def: private
    │ └─ [3] ⚙️ FUNCTION: BlueprintEncoder._encodeCore(bytes,bytes[],uint256[],bytes,uint256,uint256) (NodeID: 59)
    │     💬 Args: [blueprint, registers, dynHead, out, 32 + 4, headBytes]
    │     👁️  Def: private
    │   ├─ [4] ⚙️ FUNCTION: BlueprintEncoder._mkFrame(enum BlueprintEncoder.ContainerKind,uint256,uint256,uint256,uint256) (NodeID: 60)
    │   │   💬 Args: [ContainerKind.NONE, headStart, 0, tailStart, 0]
    │   │   👁️  Def: private
    │   ├─ [4] ⚙️ FUNCTION: BlueprintEncoder._decodeKind(uint8) (NodeID: 61)
    │   │   💬 Args: [tok]
    │   │   👁️  Def: private
    │   ├─ [4] ⚙️ FUNCTION: BlueprintEncoder._fTailOff(uint256) (NodeID: 62)
    │   │   💬 Args: [frame]
    │   │   👁️  Def: private
    │   ├─ [4] ⚙️ FUNCTION: BlueprintEncoder._fTailStart(uint256) (NodeID: 63)
    │   │   💬 Args: [frame]
    │   │   👁️  Def: private
    │   ├─ [4] ⚙️ FUNCTION: BlueprintEncoder._fHeadStart(uint256) (NodeID: 64)
    │   │   💬 Args: [frame]
    │   │   👁️  Def: private
    │   ├─ [4] ⚙️ FUNCTION: BlueprintEncoder._writeUint(bytes,uint256,uint256) (NodeID: 65)
    │   │   💬 Args: [out, _fHeadStart(frame) + _fHeadOff(frame), relPtr]
    │   │   👁️  Def: private
    │   │ ├─ [5] ⚙️ FUNCTION: BlueprintEncoder._fHeadOff(uint256) (NodeID: 66)
    │   │ │   💬 Args: [frame]
    │   │ │   👁️  Def: private
    │   │ └─ [5] ⚙️ FUNCTION: BlueprintEncoder._fHeadStart(uint256) (NodeID: 67)
    │   │     💬 Args: [frame]
    │   │     👁️  Def: private
    │   ├─ [4] ⚙️ FUNCTION: BlueprintEncoder._fBumpHead(uint256,uint256) (NodeID: 68)
    │   │   💬 Args: [frame, 32]
    │   │   👁️  Def: private
    │   ├─ [4] ⚙️ FUNCTION: BlueprintEncoder._writeUint(bytes,uint256,uint256) (NodeID: 69)
    │   │   💬 Args: [out, childHeadStart, arrLen]
    │   │   👁️  Def: private
    │   ├─ [4] ⚙️ FUNCTION: BlueprintEncoder._fBumpTail(uint256,uint256) (NodeID: 70)
    │   │   💬 Args: [frame, 32 + childHeadLen]
    │   │   👁️  Def: private
    │   ├─ [4] ⚙️ FUNCTION: BlueprintEncoder._fBumpTail(uint256,uint256) (NodeID: 71)
    │   │   💬 Args: [frame, childHeadLen]
    │   │   👁️  Def: private
    │   ├─ [4] ⚙️ FUNCTION: BlueprintEncoder._fHeadOff(uint256) (NodeID: 72)
    │   │   💬 Args: [frame]
    │   │   👁️  Def: private
    │   ├─ [4] ⚙️ FUNCTION: BlueprintEncoder._fHeadStart(uint256) (NodeID: 73)
    │   │   💬 Args: [frame]
    │   │   👁️  Def: private
    │   ├─ [4] ⚙️ FUNCTION: BlueprintEncoder._fTailOff(uint256) (NodeID: 74)
    │   │   💬 Args: [frame]
    │   │   👁️  Def: private
    │   ├─ [4] ⚙️ FUNCTION: BlueprintEncoder._fTailStart(uint256) (NodeID: 75)
    │   │   💬 Args: [frame]
    │   │   👁️  Def: private
    │   ├─ [4] ⚙️ FUNCTION: BlueprintEncoder._mkFrame(enum BlueprintEncoder.ContainerKind,uint256,uint256,uint256,uint256) (NodeID: 76)
    │   │   💬 Args: [ck, childHeadStart, childHeadOff, childTailStart, 0]
    │   │   👁️  Def: private
    │   ├─ [4] ⚙️ FUNCTION: BlueprintEncoder._fKind(uint256) (NodeID: 77)
    │   │   💬 Args: [closed]
    │   │   👁️  Def: private
    │   ├─ [4] ⚙️ FUNCTION: BlueprintEncoder._fKind(uint256) (NodeID: 78)
    │   │   💬 Args: [closed]
    │   │   👁️  Def: private
    │   ├─ [4] ⚙️ FUNCTION: BlueprintEncoder._fBumpTail(uint256,uint256) (NodeID: 79)
    │   │   💬 Args: [fs[sp], _fTailOff(closed)]
    │   │   👁️  Def: private
    │   │ └─ [5] ⚙️ FUNCTION: BlueprintEncoder._fTailOff(uint256) (NodeID: 80)
    │   │     💬 Args: [closed]
    │   │     👁️  Def: private
    │   ├─ [4] ⚙️ FUNCTION: BlueprintEncoder._fBumpHead(uint256,uint256) (NodeID: 81)
    │   │   💬 Args: [fs[sp], _fHeadOff(closed)]
    │   │   👁️  Def: private
    │   │ └─ [5] ⚙️ FUNCTION: BlueprintEncoder._fHeadOff(uint256) (NodeID: 82)
    │   │     💬 Args: [closed]
    │   │     👁️  Def: private
    │   ├─ [4] ⚙️ FUNCTION: BlueprintEncoder._fBumpTail(uint256,uint256) (NodeID: 83)
    │   │   💬 Args: [fs[sp], _fTailOff(closed)]
    │   │   👁️  Def: private
    │   │ └─ [5] ⚙️ FUNCTION: BlueprintEncoder._fTailOff(uint256) (NodeID: 84)
    │   │     💬 Args: [closed]
    │   │     👁️  Def: private
    │   ├─ [4] ⚙️ FUNCTION: RegisterFile.get(bytes[],uint256) (NodeID: 85)
    │   │   💬 Args: [registers, idx]
    │   │   👁️  Def: internal
    │   ├─ [4] ⚙️ FUNCTION: BlueprintEncoder._fHeadOff(uint256) (NodeID: 86)
    │   │   💬 Args: [frame]
    │   │   👁️  Def: private
    │   ├─ [4] ⚙️ FUNCTION: BlueprintEncoder._fHeadStart(uint256) (NodeID: 87)
    │   │   💬 Args: [frame]
    │   │   👁️  Def: private
    │   ├─ [4] ⚙️ FUNCTION: BlueprintEncoder._copyWord(bytes,uint256,bytes) (NodeID: 88)
    │   │   💬 Args: [out, dst, registers.get(idx)]
    │   │   👁️  Def: private
    │   │ └─ [5] ⚙️ FUNCTION: RegisterFile.get(bytes[],uint256) (NodeID: 89)
    │   │     💬 Args: [registers, idx]
    │   │     👁️  Def: internal
    │   ├─ [4] ⚙️ FUNCTION: BlueprintEncoder._fBumpHead(uint256,uint256) (NodeID: 90)
    │   │   💬 Args: [frame, 32]
    │   │   👁️  Def: private
    │   ├─ [4] ⚙️ FUNCTION: RegisterHelpers.isDyn(uint8) (NodeID: 91)
    │   │   💬 Args: [tok]
    │   │   👁️  Def: internal
    │   ├─ [4] ⚙️ FUNCTION: RegisterHelpers.idx(uint8) (NodeID: 92)
    │   │   💬 Args: [tok]
    │   │   👁️  Def: internal
    │   ├─ [4] ⚙️ FUNCTION: RegisterFile.get(bytes[],uint256) (NodeID: 93)
    │   │   💬 Args: [registers, idx]
    │   │   👁️  Def: internal
    │   ├─ [4] ⚙️ FUNCTION: BlueprintEncoder._fTailOff(uint256) (NodeID: 94)
    │   │   💬 Args: [frame]
    │   │   👁️  Def: private
    │   ├─ [4] ⚙️ FUNCTION: BlueprintEncoder._fTailStart(uint256) (NodeID: 95)
    │   │   💬 Args: [frame]
    │   │   👁️  Def: private
    │   ├─ [4] ⚙️ FUNCTION: BlueprintEncoder._fHeadStart(uint256) (NodeID: 96)
    │   │   💬 Args: [frame]
    │   │   👁️  Def: private
    │   ├─ [4] ⚙️ FUNCTION: BlueprintEncoder._writeUint(bytes,uint256,uint256) (NodeID: 97)
    │   │   💬 Args: [out, _fHeadStart(frame) + _fHeadOff(frame), relPtr]
    │   │   👁️  Def: private
    │   │ ├─ [5] ⚙️ FUNCTION: BlueprintEncoder._fHeadOff(uint256) (NodeID: 98)
    │   │ │   💬 Args: [frame]
    │   │ │   👁️  Def: private
    │   │ └─ [5] ⚙️ FUNCTION: BlueprintEncoder._fHeadStart(uint256) (NodeID: 99)
    │   │     💬 Args: [frame]
    │   │     👁️  Def: private
    │   ├─ [4] ⚙️ FUNCTION: BlueprintEncoder._copyDyn(bytes,uint256,bytes) (NodeID: 100)
    │   │   💬 Args: [out, absPtr, registers.get(idx)]
    │   │   👁️  Def: private
    │   │ └─ [5] ⚙️ FUNCTION: RegisterFile.get(bytes[],uint256) (NodeID: 101)
    │   │     💬 Args: [registers, idx]
    │   │     👁️  Def: internal
    │   ├─ [4] ⚙️ FUNCTION: BlueprintEncoder._fBumpHead(uint256,uint256) (NodeID: 102)
    │   │   💬 Args: [frame, 32]
    │   │   👁️  Def: private
    │   └─ [4] ⚙️ FUNCTION: BlueprintEncoder._fBumpTail(uint256,uint256) (NodeID: 103)
    │       💬 Args: [fs[sp], bytesW]
    │       👁️  Def: private
    ├─ [2] ⚙️ FUNCTION: RegisterFile.set(bytes[],uint256,bytes) (NodeID: 104)
    │   💬 Args: [vmState.registers, cdb.destReg.idx(), built]
    │   👁️  Def: internal
    │ └─ [3] ⚙️ FUNCTION: RegisterHelpers.idx(uint8) (NodeID: 105)
    │     💬 Args: [cdb.destReg]
    │     👁️  Def: internal
    ├─ [2] ⚙️ FUNCTION: CommandPacking.unpackDepositApproved(bytes32) (NodeID: 106)
    │   💬 Args: [data]
    │   👁️  Def: internal
    ├─ [2] ⚙️ FUNCTION: DepositApprovedLib.depositApproved(struct DepositApproved) (NodeID: 107)
    │   💬 Args: [d]
    │   👁️  Def: internal
    ├─ [2] ⚙️ FUNCTION: RegisterFile.set(bytes[],uint256,bytes) (NodeID: 108)
    │   💬 Args: [vmState.registers, d.destReg.idx(), RegisterHelpers.encUint(depositedAmount)]
    │   👁️  Def: internal
    │ ├─ [3] ⚙️ FUNCTION: RegisterHelpers.idx(uint8) (NodeID: 109)
    │ │   💬 Args: [d.destReg]
    │ │   👁️  Def: internal
    │ └─ [3] ⚙️ FUNCTION: RegisterHelpers.encUint(uint256) (NodeID: 110)
    │     💬 Args: [depositedAmount]
    │     👁️  Def: internal
    ├─ [2] ⚙️ FUNCTION: CommandPacking.unpackCallDataSurgery(bytes32) (NodeID: 111)
    │   💬 Args: [data]
    │   👁️  Def: internal
    ├─ [2] ⚙️ FUNCTION: SurgeryOps.performSurgery(struct VMState,uint8,struct SurgeryDescriptor[6],uint8) (NodeID: 112)
    │   💬 Args: [vmState, s.sourceReg, s.surgeries, s.surgeryCount]
    │   👁️  Def: internal
    │ ├─ [3] ⚙️ FUNCTION: RegisterHelpers.idx(uint8) (NodeID: 113)
    │ │   💬 Args: [sourceReg]
    │ │   👁️  Def: internal
    │ └─ [3] ⚙️ FUNCTION: RegisterHelpers.idx(uint8) (NodeID: 114)
    │     💬 Args: [desc.replacementReg]
    │     👁️  Def: internal
    ├─ [2] ⚙️ FUNCTION: CommandPacking.unpackReturn(bytes32) (NodeID: 115)
    │   💬 Args: [data]
    │   👁️  Def: internal
    ├─ [2] ⚙️ FUNCTION: RegisterFile.get(bytes[],uint256) (NodeID: 116)
    │   💬 Args: [vmState.registers, r.sourceReg.idx()]
    │   👁️  Def: internal
    │ └─ [3] ⚙️ FUNCTION: RegisterHelpers.idx(uint8) (NodeID: 117)
    │     💬 Args: [r.sourceReg]
    │     👁️  Def: internal
    ├─ [2] ⚙️ FUNCTION: CommandPacking.unpackAbiEncode(bytes32) (NodeID: 118)
    │   💬 Args: [data]
    │   👁️  Def: internal
    ├─ [2] ⚙️ FUNCTION: BlueprintEncoder.encodeData(bytes,bytes[]) (NodeID: 119)
    │   💬 Args: [ae.blueprint, vmState.registers]
    │   👁️  Def: public
    │ ├─ [3] ⚙️ FUNCTION: BlueprintEncoder._allocWithLength(uint256) (NodeID: 120)
    │ │   💬 Args: [0]
    │ │   👁️  Def: private
    │ │ └─ [4] ⚙️ FUNCTION: BlueprintEncoder._writeUint(bytes,uint256,uint256) (NodeID: 121)
    │ │     💬 Args: [out, 0, payloadLen]
    │ │     👁️  Def: private
    │ ├─ [3] ⚙️ FUNCTION: BlueprintEncoder._isFlat(bytes) (NodeID: 122)
    │ │   💬 Args: [blueprint]
    │ │   👁️  Def: private
    │ ├─ [3] ⚙️ FUNCTION: BlueprintEncoder._measureFlat(bytes,bytes[]) (NodeID: 123)
    │ │   💬 Args: [blueprint, registers]
    │ │   👁️  Def: private
    │ │ ├─ [4] ⚙️ FUNCTION: RegisterHelpers.isDyn(uint8) (NodeID: 124)
    │ │ │   💬 Args: [tok]
    │ │ │   👁️  Def: internal
    │ │ ├─ [4] ⚙️ FUNCTION: RegisterHelpers.idx(uint8) (NodeID: 125)
    │ │ │   💬 Args: [tok]
    │ │ │   👁️  Def: internal
    │ │ ├─ [4] ⚙️ FUNCTION: RegisterFile.get(bytes[],uint256) (NodeID: 126)
    │ │ │   💬 Args: [regs, idx]
    │ │ │   👁️  Def: internal
    │ │ └─ [4] ⚙️ FUNCTION: RegisterFile.get(bytes[],uint256) (NodeID: 127)
    │ │     💬 Args: [regs, tok]
    │ │     👁️  Def: internal
    │ ├─ [3] ⚙️ FUNCTION: BlueprintEncoder._allocWithLength(uint256) (NodeID: 128)
    │ │   💬 Args: [payloadLen]
    │ │   👁️  Def: private
    │ │ └─ [4] ⚙️ FUNCTION: BlueprintEncoder._writeUint(bytes,uint256,uint256) (NodeID: 129)
    │ │     💬 Args: [out, 0, payloadLen]
    │ │     👁️  Def: private
    │ ├─ [3] ⚙️ FUNCTION: BlueprintEncoder._encodeFlatCoreInto(bytes,bytes,bytes[],uint256,uint256) (NodeID: 130)
    │ │   💬 Args: [out, blueprint, registers, headB, 32]
    │ │   👁️  Def: private
    │ │ ├─ [4] ⚙️ FUNCTION: RegisterFile.get(bytes[],uint256) (NodeID: 131)
    │ │ │   💬 Args: [regs, idx]
    │ │ │   👁️  Def: internal
    │ │ ├─ [4] ⚙️ FUNCTION: BlueprintEncoder._copyWord(bytes,uint256,bytes) (NodeID: 132)
    │ │ │   💬 Args: [out, headBase + headPtr, regs.get(idx)]
    │ │ │   👁️  Def: private
    │ │ │ └─ [5] ⚙️ FUNCTION: RegisterFile.get(bytes[],uint256) (NodeID: 133)
    │ │ │     💬 Args: [regs, idx]
    │ │ │     👁️  Def: internal
    │ │ ├─ [4] ⚙️ FUNCTION: RegisterHelpers.idx(uint8) (NodeID: 134)
    │ │ │   💬 Args: [tok]
    │ │ │   👁️  Def: internal
    │ │ ├─ [4] ⚙️ FUNCTION: RegisterFile.get(bytes[],uint256) (NodeID: 135)
    │ │ │   💬 Args: [regs, idx]
    │ │ │   👁️  Def: internal
    │ │ ├─ [4] ⚙️ FUNCTION: BlueprintEncoder._writeUint(bytes,uint256,uint256) (NodeID: 136)
    │ │ │   💬 Args: [out, headBase + headPtr, relPtr]
    │ │ │   👁️  Def: private
    │ │ └─ [4] ⚙️ FUNCTION: BlueprintEncoder._copyDyn(bytes,uint256,bytes) (NodeID: 137)
    │ │     💬 Args: [out, absTail, reg]
    │ │     👁️  Def: private
    │ ├─ [3] ⚙️ FUNCTION: BlueprintEncoder._measure(bytes,bytes[],uint256[10]) (NodeID: 138)
    │ │   💬 Args: [blueprint, registers, tmpHeads]
    │ │   👁️  Def: private
    │ │ ├─ [4] ⚙️ FUNCTION: BlueprintEncoder._mkSizing(uint256,uint256,enum BlueprintEncoder.ContainerKind) (NodeID: 139)
    │ │ │   💬 Args: [0, 0, ContainerKind.NONE]
    │ │ │   👁️  Def: private
    │ │ ├─ [4] ⚙️ FUNCTION: BlueprintEncoder._decodeKind(uint8) (NodeID: 140)
    │ │ │   💬 Args: [tok]
    │ │ │   👁️  Def: private
    │ │ ├─ [4] ⚙️ FUNCTION: BlueprintEncoder._mkSizing(uint256,uint256,enum BlueprintEncoder.ContainerKind) (NodeID: 141)
    │ │ │   💬 Args: [0, 0, open]
    │ │ │   👁️  Def: private
    │ │ ├─ [4] ⚙️ FUNCTION: BlueprintEncoder._szKind(uint256) (NodeID: 142)
    │ │ │   💬 Args: [child]
    │ │ │   👁️  Def: private
    │ │ ├─ [4] ⚙️ FUNCTION: BlueprintEncoder._szHead(uint256) (NodeID: 143)
    │ │ │   💬 Args: [child]
    │ │ │   👁️  Def: private
    │ │ ├─ [4] ⚙️ FUNCTION: BlueprintEncoder._szTail(uint256) (NodeID: 144)
    │ │ │   💬 Args: [child]
    │ │ │   👁️  Def: private
    │ │ ├─ [4] ⚙️ FUNCTION: BlueprintEncoder._szKind(uint256) (NodeID: 145)
    │ │ │   💬 Args: [stack[sp]]
    │ │ │   👁️  Def: private
    │ │ ├─ [4] ⚙️ FUNCTION: BlueprintEncoder._szAddHead(uint256,uint256) (NodeID: 146)
    │ │ │   💬 Args: [stack[sp], 32]
    │ │ │   👁️  Def: private
    │ │ ├─ [4] ⚙️ FUNCTION: BlueprintEncoder._szAddTail(uint256,uint256) (NodeID: 147)
    │ │ │   💬 Args: [stack[sp], bytesChild + 32]
    │ │ │   👁️  Def: private
    │ │ ├─ [4] ⚙️ FUNCTION: BlueprintEncoder._szAddTail(uint256,uint256) (NodeID: 148)
    │ │ │   💬 Args: [stack[sp], bytesChild]
    │ │ │   👁️  Def: private
    │ │ ├─ [4] ⚙️ FUNCTION: BlueprintEncoder._szAddHead(uint256,uint256) (NodeID: 149)
    │ │ │   💬 Args: [stack[sp], childHeadLen]
    │ │ │   👁️  Def: private
    │ │ ├─ [4] ⚙️ FUNCTION: BlueprintEncoder._szAddTail(uint256,uint256) (NodeID: 150)
    │ │ │   💬 Args: [stack[sp], childTailLen]
    │ │ │   👁️  Def: private
    │ │ ├─ [4] ⚙️ FUNCTION: BlueprintEncoder._szAddHead(uint256,uint256) (NodeID: 151)
    │ │ │   💬 Args: [stack[sp], 32]
    │ │ │   👁️  Def: private
    │ │ ├─ [4] ⚙️ FUNCTION: BlueprintEncoder._szKind(uint256) (NodeID: 152)
    │ │ │   💬 Args: [stack[sp]]
    │ │ │   👁️  Def: private
    │ │ ├─ [4] ⚙️ FUNCTION: RegisterHelpers.isDyn(uint8) (NodeID: 153)
    │ │ │   💬 Args: [tok]
    │ │ │   👁️  Def: internal
    │ │ ├─ [4] ⚙️ FUNCTION: RegisterHelpers.idx(uint8) (NodeID: 154)
    │ │ │   💬 Args: [tok]
    │ │ │   👁️  Def: internal
    │ │ ├─ [4] ⚙️ FUNCTION: RegisterFile.get(bytes[],uint256) (NodeID: 155)
    │ │ │   💬 Args: [registers, idx]
    │ │ │   👁️  Def: internal
    │ │ ├─ [4] ⚙️ FUNCTION: BlueprintEncoder._szAddHead(uint256,uint256) (NodeID: 156)
    │ │ │   💬 Args: [stack[sp], 32]
    │ │ │   👁️  Def: private
    │ │ ├─ [4] ⚙️ FUNCTION: BlueprintEncoder._szAddTail(uint256,uint256) (NodeID: 157)
    │ │ │   💬 Args: [stack[sp], blobLen]
    │ │ │   👁️  Def: private
    │ │ ├─ [4] ⚙️ FUNCTION: BlueprintEncoder._szKind(uint256) (NodeID: 158)
    │ │ │   💬 Args: [stack[sp]]
    │ │ │   👁️  Def: private
    │ │ ├─ [4] ⚙️ FUNCTION: BlueprintEncoder._szHead(uint256) (NodeID: 159)
    │ │ │   💬 Args: [stack[0]]
    │ │ │   👁️  Def: private
    │ │ └─ [4] ⚙️ FUNCTION: BlueprintEncoder._szTail(uint256) (NodeID: 160)
    │ │     💬 Args: [stack[0]]
    │ │     👁️  Def: private
    │ ├─ [3] ⚙️ FUNCTION: BlueprintEncoder._allocWithLength(uint256) (NodeID: 161)
    │ │   💬 Args: [total]
    │ │   👁️  Def: private
    │ │ └─ [4] ⚙️ FUNCTION: BlueprintEncoder._writeUint(bytes,uint256,uint256) (NodeID: 162)
    │ │     💬 Args: [out, 0, payloadLen]
    │ │     👁️  Def: private
    │ └─ [3] ⚙️ FUNCTION: BlueprintEncoder._encodeCore(bytes,bytes[],uint256[],bytes,uint256,uint256) (NodeID: 163)
    │     💬 Args: [blueprint, registers, dynHead, out, 32, headBytes]
    │     👁️  Def: private
    │   ├─ [4] ⚙️ FUNCTION: BlueprintEncoder._mkFrame(enum BlueprintEncoder.ContainerKind,uint256,uint256,uint256,uint256) (NodeID: 164)
    │   │   💬 Args: [ContainerKind.NONE, headStart, 0, tailStart, 0]
    │   │   👁️  Def: private
    │   ├─ [4] ⚙️ FUNCTION: BlueprintEncoder._decodeKind(uint8) (NodeID: 165)
    │   │   💬 Args: [tok]
    │   │   👁️  Def: private
    │   ├─ [4] ⚙️ FUNCTION: BlueprintEncoder._fTailOff(uint256) (NodeID: 166)
    │   │   💬 Args: [frame]
    │   │   👁️  Def: private
    │   ├─ [4] ⚙️ FUNCTION: BlueprintEncoder._fTailStart(uint256) (NodeID: 167)
    │   │   💬 Args: [frame]
    │   │   👁️  Def: private
    │   ├─ [4] ⚙️ FUNCTION: BlueprintEncoder._fHeadStart(uint256) (NodeID: 168)
    │   │   💬 Args: [frame]
    │   │   👁️  Def: private
    │   ├─ [4] ⚙️ FUNCTION: BlueprintEncoder._writeUint(bytes,uint256,uint256) (NodeID: 169)
    │   │   💬 Args: [out, _fHeadStart(frame) + _fHeadOff(frame), relPtr]
    │   │   👁️  Def: private
    │   │ ├─ [5] ⚙️ FUNCTION: BlueprintEncoder._fHeadOff(uint256) (NodeID: 170)
    │   │ │   💬 Args: [frame]
    │   │ │   👁️  Def: private
    │   │ └─ [5] ⚙️ FUNCTION: BlueprintEncoder._fHeadStart(uint256) (NodeID: 171)
    │   │     💬 Args: [frame]
    │   │     👁️  Def: private
    │   ├─ [4] ⚙️ FUNCTION: BlueprintEncoder._fBumpHead(uint256,uint256) (NodeID: 172)
    │   │   💬 Args: [frame, 32]
    │   │   👁️  Def: private
    │   ├─ [4] ⚙️ FUNCTION: BlueprintEncoder._writeUint(bytes,uint256,uint256) (NodeID: 173)
    │   │   💬 Args: [out, childHeadStart, arrLen]
    │   │   👁️  Def: private
    │   ├─ [4] ⚙️ FUNCTION: BlueprintEncoder._fBumpTail(uint256,uint256) (NodeID: 174)
    │   │   💬 Args: [frame, 32 + childHeadLen]
    │   │   👁️  Def: private
    │   ├─ [4] ⚙️ FUNCTION: BlueprintEncoder._fBumpTail(uint256,uint256) (NodeID: 175)
    │   │   💬 Args: [frame, childHeadLen]
    │   │   👁️  Def: private
    │   ├─ [4] ⚙️ FUNCTION: BlueprintEncoder._fHeadOff(uint256) (NodeID: 176)
    │   │   💬 Args: [frame]
    │   │   👁️  Def: private
    │   ├─ [4] ⚙️ FUNCTION: BlueprintEncoder._fHeadStart(uint256) (NodeID: 177)
    │   │   💬 Args: [frame]
    │   │   👁️  Def: private
    │   ├─ [4] ⚙️ FUNCTION: BlueprintEncoder._fTailOff(uint256) (NodeID: 178)
    │   │   💬 Args: [frame]
    │   │   👁️  Def: private
    │   ├─ [4] ⚙️ FUNCTION: BlueprintEncoder._fTailStart(uint256) (NodeID: 179)
    │   │   💬 Args: [frame]
    │   │   👁️  Def: private
    │   ├─ [4] ⚙️ FUNCTION: BlueprintEncoder._mkFrame(enum BlueprintEncoder.ContainerKind,uint256,uint256,uint256,uint256) (NodeID: 180)
    │   │   💬 Args: [ck, childHeadStart, childHeadOff, childTailStart, 0]
    │   │   👁️  Def: private
    │   ├─ [4] ⚙️ FUNCTION: BlueprintEncoder._fKind(uint256) (NodeID: 181)
    │   │   💬 Args: [closed]
    │   │   👁️  Def: private
    │   ├─ [4] ⚙️ FUNCTION: BlueprintEncoder._fKind(uint256) (NodeID: 182)
    │   │   💬 Args: [closed]
    │   │   👁️  Def: private
    │   ├─ [4] ⚙️ FUNCTION: BlueprintEncoder._fBumpTail(uint256,uint256) (NodeID: 183)
    │   │   💬 Args: [fs[sp], _fTailOff(closed)]
    │   │   👁️  Def: private
    │   │ └─ [5] ⚙️ FUNCTION: BlueprintEncoder._fTailOff(uint256) (NodeID: 184)
    │   │     💬 Args: [closed]
    │   │     👁️  Def: private
    │   ├─ [4] ⚙️ FUNCTION: BlueprintEncoder._fBumpHead(uint256,uint256) (NodeID: 185)
    │   │   💬 Args: [fs[sp], _fHeadOff(closed)]
    │   │   👁️  Def: private
    │   │ └─ [5] ⚙️ FUNCTION: BlueprintEncoder._fHeadOff(uint256) (NodeID: 186)
    │   │     💬 Args: [closed]
    │   │     👁️  Def: private
    │   ├─ [4] ⚙️ FUNCTION: BlueprintEncoder._fBumpTail(uint256,uint256) (NodeID: 187)
    │   │   💬 Args: [fs[sp], _fTailOff(closed)]
    │   │   👁️  Def: private
    │   │ └─ [5] ⚙️ FUNCTION: BlueprintEncoder._fTailOff(uint256) (NodeID: 188)
    │   │     💬 Args: [closed]
    │   │     👁️  Def: private
    │   ├─ [4] ⚙️ FUNCTION: RegisterFile.get(bytes[],uint256) (NodeID: 189)
    │   │   💬 Args: [registers, idx]
    │   │   👁️  Def: internal
    │   ├─ [4] ⚙️ FUNCTION: BlueprintEncoder._fHeadOff(uint256) (NodeID: 190)
    │   │   💬 Args: [frame]
    │   │   👁️  Def: private
    │   ├─ [4] ⚙️ FUNCTION: BlueprintEncoder._fHeadStart(uint256) (NodeID: 191)
    │   │   💬 Args: [frame]
    │   │   👁️  Def: private
    │   ├─ [4] ⚙️ FUNCTION: BlueprintEncoder._copyWord(bytes,uint256,bytes) (NodeID: 192)
    │   │   💬 Args: [out, dst, registers.get(idx)]
    │   │   👁️  Def: private
    │   │ └─ [5] ⚙️ FUNCTION: RegisterFile.get(bytes[],uint256) (NodeID: 193)
    │   │     💬 Args: [registers, idx]
    │   │     👁️  Def: internal
    │   ├─ [4] ⚙️ FUNCTION: BlueprintEncoder._fBumpHead(uint256,uint256) (NodeID: 194)
    │   │   💬 Args: [frame, 32]
    │   │   👁️  Def: private
    │   ├─ [4] ⚙️ FUNCTION: RegisterHelpers.isDyn(uint8) (NodeID: 195)
    │   │   💬 Args: [tok]
    │   │   👁️  Def: internal
    │   ├─ [4] ⚙️ FUNCTION: RegisterHelpers.idx(uint8) (NodeID: 196)
    │   │   💬 Args: [tok]
    │   │   👁️  Def: internal
    │   ├─ [4] ⚙️ FUNCTION: RegisterFile.get(bytes[],uint256) (NodeID: 197)
    │   │   💬 Args: [registers, idx]
    │   │   👁️  Def: internal
    │   ├─ [4] ⚙️ FUNCTION: BlueprintEncoder._fTailOff(uint256) (NodeID: 198)
    │   │   💬 Args: [frame]
    │   │   👁️  Def: private
    │   ├─ [4] ⚙️ FUNCTION: BlueprintEncoder._fTailStart(uint256) (NodeID: 199)
    │   │   💬 Args: [frame]
    │   │   👁️  Def: private
    │   ├─ [4] ⚙️ FUNCTION: BlueprintEncoder._fHeadStart(uint256) (NodeID: 200)
    │   │   💬 Args: [frame]
    │   │   👁️  Def: private
    │   ├─ [4] ⚙️ FUNCTION: BlueprintEncoder._writeUint(bytes,uint256,uint256) (NodeID: 201)
    │   │   💬 Args: [out, _fHeadStart(frame) + _fHeadOff(frame), relPtr]
    │   │   👁️  Def: private
    │   │ ├─ [5] ⚙️ FUNCTION: BlueprintEncoder._fHeadOff(uint256) (NodeID: 202)
    │   │ │   💬 Args: [frame]
    │   │ │   👁️  Def: private
    │   │ └─ [5] ⚙️ FUNCTION: BlueprintEncoder._fHeadStart(uint256) (NodeID: 203)
    │   │     💬 Args: [frame]
    │   │     👁️  Def: private
    │   ├─ [4] ⚙️ FUNCTION: BlueprintEncoder._copyDyn(bytes,uint256,bytes) (NodeID: 204)
    │   │   💬 Args: [out, absPtr, registers.get(idx)]
    │   │   👁️  Def: private
    │   │ └─ [5] ⚙️ FUNCTION: RegisterFile.get(bytes[],uint256) (NodeID: 205)
    │   │     💬 Args: [registers, idx]
    │   │     👁️  Def: internal
    │   ├─ [4] ⚙️ FUNCTION: BlueprintEncoder._fBumpHead(uint256,uint256) (NodeID: 206)
    │   │   💬 Args: [frame, 32]
    │   │   👁️  Def: private
    │   └─ [4] ⚙️ FUNCTION: BlueprintEncoder._fBumpTail(uint256,uint256) (NodeID: 207)
    │       💬 Args: [fs[sp], bytesW]
    │       👁️  Def: private
    ├─ [2] ⚙️ FUNCTION: RegisterFile.set(bytes[],uint256,bytes) (NodeID: 208)
    │   💬 Args: [vmState.registers, ae.destReg.idx(), enc]
    │   👁️  Def: internal
    │ └─ [3] ⚙️ FUNCTION: RegisterHelpers.idx(uint8) (NodeID: 209)
    │     💬 Args: [ae.destReg]
    │     👁️  Def: internal
    ├─ [2] ⚙️ FUNCTION: CommandPacking.unpackRemainingGas(bytes32) (NodeID: 210)
    │   💬 Args: [data]
    │   👁️  Def: internal
    ├─ [2] ⚙️ FUNCTION: RegisterFile.set(bytes[],uint256,bytes) (NodeID: 211)
    │   💬 Args: [vmState.registers, rg.destReg.idx(), RegisterHelpers.encUint(gasleft())]
    │   👁️  Def: internal
    │ ├─ [3] ⚙️ FUNCTION: RegisterHelpers.idx(uint8) (NodeID: 212)
    │ │   💬 Args: [rg.destReg]
    │ │   👁️  Def: internal
    │ └─ [3] ⚙️ FUNCTION: RegisterHelpers.encUint(uint256) (NodeID: 213)
    │     💬 Args: [gasleft()]
    │     👁️  Def: internal
    ├─ [2] ⚙️ FUNCTION: CommandPacking.unpackNativeBalance(bytes32) (NodeID: 214)
    │   💬 Args: [data]
    │   👁️  Def: internal
    ├─ [2] ⚙️ FUNCTION: RegisterFile.get(bytes[],uint256) (NodeID: 215)
    │   💬 Args: [vmState.registers, nb.addrReg.idx()]
    │   👁️  Def: internal
    │ └─ [3] ⚙️ FUNCTION: RegisterHelpers.idx(uint8) (NodeID: 216)
    │     💬 Args: [nb.addrReg]
    │     👁️  Def: internal
    ├─ [2] ⚙️ FUNCTION: RegisterHelpers.asAddress(bytes) (NodeID: 217)
    │   💬 Args: [vmState.registers.get(nb.addrReg.idx())]
    │   👁️  Def: internal
    ├─ [2] ⚙️ FUNCTION: RegisterFile.set(bytes[],uint256,bytes) (NodeID: 218)
    │   💬 Args: [vmState.registers, nb.destReg.idx(), RegisterHelpers.encUint(targetAddress.balance)]
    │   👁️  Def: internal
    │ ├─ [3] ⚙️ FUNCTION: RegisterHelpers.idx(uint8) (NodeID: 219)
    │ │   💬 Args: [nb.destReg]
    │ │   👁️  Def: internal
    │ └─ [3] ⚙️ FUNCTION: RegisterHelpers.encUint(uint256) (NodeID: 220)
    │     💬 Args: [targetAddress.balance]
    │     👁️  Def: internal
    ├─ [2] ⚙️ FUNCTION: CommandPacking.unpackLog(bytes32) (NodeID: 221)
    │   💬 Args: [data]
    │   👁️  Def: internal
    └─ [2] ⚙️ FUNCTION: VMLogLib.execute(struct VMState,struct Log) (NodeID: 222)
        💬 Args: [vmState, log]
        👁️  Def: internal
      ├─ [3] ⚙️ FUNCTION: VMLogLib.executeStatic1(bytes[],uint256) (NodeID: 223)
      │   💬 Args: [vmState.registers, cmd.sourceRegs]
      │   👁️  Def: private
      │ ├─ [4] ⚙️ FUNCTION: VMLogLib.extractRegister(uint256,uint8) (NodeID: 224)
      │ │   💬 Args: [sourceRegs, 0]
      │ │   👁️  Def: private
      │ └─ [4] ⚙️ FUNCTION: RegisterFile.getStatic(bytes[],uint256) (NodeID: 225)
      │     💬 Args: [registers, reg1.idx()]
      │     👁️  Def: internal
      │   └─ [5] ⚙️ FUNCTION: RegisterHelpers.idx(uint8) (NodeID: 226)
      │       💬 Args: [reg1]
      │       👁️  Def: internal
      ├─ [3] ⚙️ FUNCTION: VMLogLib.executeStatic2(bytes[],uint256) (NodeID: 227)
      │   💬 Args: [vmState.registers, cmd.sourceRegs]
      │   👁️  Def: private
      │ ├─ [4] ⚙️ FUNCTION: VMLogLib.extractRegister(uint256,uint8) (NodeID: 228)
      │ │   💬 Args: [sourceRegs, 0]
      │ │   👁️  Def: private
      │ ├─ [4] ⚙️ FUNCTION: VMLogLib.extractRegister(uint256,uint8) (NodeID: 229)
      │ │   💬 Args: [sourceRegs, 1]
      │ │   👁️  Def: private
      │ ├─ [4] ⚙️ FUNCTION: RegisterFile.getStatic(bytes[],uint256) (NodeID: 230)
      │ │   💬 Args: [registers, reg1.idx()]
      │ │   👁️  Def: internal
      │ │ └─ [5] ⚙️ FUNCTION: RegisterHelpers.idx(uint8) (NodeID: 231)
      │ │     💬 Args: [reg1]
      │ │     👁️  Def: internal
      │ └─ [4] ⚙️ FUNCTION: RegisterFile.getStatic(bytes[],uint256) (NodeID: 232)
      │     💬 Args: [registers, reg2.idx()]
      │     👁️  Def: internal
      │   └─ [5] ⚙️ FUNCTION: RegisterHelpers.idx(uint8) (NodeID: 233)
      │       💬 Args: [reg2]
      │       👁️  Def: internal
      ├─ [3] ⚙️ FUNCTION: VMLogLib.executeStatic3(bytes[],uint256) (NodeID: 234)
      │   💬 Args: [vmState.registers, cmd.sourceRegs]
      │   👁️  Def: private
      │ ├─ [4] ⚙️ FUNCTION: VMLogLib.extractRegister(uint256,uint8) (NodeID: 235)
      │ │   💬 Args: [sourceRegs, 0]
      │ │   👁️  Def: private
      │ ├─ [4] ⚙️ FUNCTION: VMLogLib.extractRegister(uint256,uint8) (NodeID: 236)
      │ │   💬 Args: [sourceRegs, 1]
      │ │   👁️  Def: private
      │ ├─ [4] ⚙️ FUNCTION: VMLogLib.extractRegister(uint256,uint8) (NodeID: 237)
      │ │   💬 Args: [sourceRegs, 2]
      │ │   👁️  Def: private
      │ ├─ [4] ⚙️ FUNCTION: RegisterFile.getStatic(bytes[],uint256) (NodeID: 238)
      │ │   💬 Args: [registers, reg1.idx()]
      │ │   👁️  Def: internal
      │ │ └─ [5] ⚙️ FUNCTION: RegisterHelpers.idx(uint8) (NodeID: 239)
      │ │     💬 Args: [reg1]
      │ │     👁️  Def: internal
      │ ├─ [4] ⚙️ FUNCTION: RegisterFile.getStatic(bytes[],uint256) (NodeID: 240)
      │ │   💬 Args: [registers, reg2.idx()]
      │ │   👁️  Def: internal
      │ │ └─ [5] ⚙️ FUNCTION: RegisterHelpers.idx(uint8) (NodeID: 241)
      │ │     💬 Args: [reg2]
      │ │     👁️  Def: internal
      │ └─ [4] ⚙️ FUNCTION: RegisterFile.getStatic(bytes[],uint256) (NodeID: 242)
      │     💬 Args: [registers, reg3.idx()]
      │     👁️  Def: internal
      │   └─ [5] ⚙️ FUNCTION: RegisterHelpers.idx(uint8) (NodeID: 243)
      │       💬 Args: [reg3]
      │       👁️  Def: internal
      ├─ [3] ⚙️ FUNCTION: VMLogLib.executeStatic4(bytes[],uint256) (NodeID: 244)
      │   💬 Args: [vmState.registers, cmd.sourceRegs]
      │   👁️  Def: private
      │ ├─ [4] ⚙️ FUNCTION: VMLogLib.extractRegister(uint256,uint8) (NodeID: 245)
      │ │   💬 Args: [sourceRegs, 0]
      │ │   👁️  Def: private
      │ ├─ [4] ⚙️ FUNCTION: VMLogLib.extractRegister(uint256,uint8) (NodeID: 246)
      │ │   💬 Args: [sourceRegs, 1]
      │ │   👁️  Def: private
      │ ├─ [4] ⚙️ FUNCTION: VMLogLib.extractRegister(uint256,uint8) (NodeID: 247)
      │ │   💬 Args: [sourceRegs, 2]
      │ │   👁️  Def: private
      │ ├─ [4] ⚙️ FUNCTION: VMLogLib.extractRegister(uint256,uint8) (NodeID: 248)
      │ │   💬 Args: [sourceRegs, 3]
      │ │   👁️  Def: private
      │ ├─ [4] ⚙️ FUNCTION: RegisterFile.getStatic(bytes[],uint256) (NodeID: 249)
      │ │   💬 Args: [registers, reg1.idx()]
      │ │   👁️  Def: internal
      │ │ └─ [5] ⚙️ FUNCTION: RegisterHelpers.idx(uint8) (NodeID: 250)
      │ │     💬 Args: [reg1]
      │ │     👁️  Def: internal
      │ ├─ [4] ⚙️ FUNCTION: RegisterFile.getStatic(bytes[],uint256) (NodeID: 251)
      │ │   💬 Args: [registers, reg2.idx()]
      │ │   👁️  Def: internal
      │ │ └─ [5] ⚙️ FUNCTION: RegisterHelpers.idx(uint8) (NodeID: 252)
      │ │     💬 Args: [reg2]
      │ │     👁️  Def: internal
      │ ├─ [4] ⚙️ FUNCTION: RegisterFile.getStatic(bytes[],uint256) (NodeID: 253)
      │ │   💬 Args: [registers, reg3.idx()]
      │ │   👁️  Def: internal
      │ │ └─ [5] ⚙️ FUNCTION: RegisterHelpers.idx(uint8) (NodeID: 254)
      │ │     💬 Args: [reg3]
      │ │     👁️  Def: internal
      │ └─ [4] ⚙️ FUNCTION: RegisterFile.getStatic(bytes[],uint256) (NodeID: 255)
      │     💬 Args: [registers, reg4.idx()]
      │     👁️  Def: internal
      │   └─ [5] ⚙️ FUNCTION: RegisterHelpers.idx(uint8) (NodeID: 256)
      │       💬 Args: [reg4]
      │       👁️  Def: internal
      ├─ [3] ⚙️ FUNCTION: VMLogLib.executeStatic5(bytes[],uint256) (NodeID: 257)
      │   💬 Args: [vmState.registers, cmd.sourceRegs]
      │   👁️  Def: private
      │ ├─ [4] ⚙️ FUNCTION: VMLogLib.extractRegister(uint256,uint8) (NodeID: 258)
      │ │   💬 Args: [sourceRegs, 0]
      │ │   👁️  Def: private
      │ ├─ [4] ⚙️ FUNCTION: VMLogLib.extractRegister(uint256,uint8) (NodeID: 259)
      │ │   💬 Args: [sourceRegs, 1]
      │ │   👁️  Def: private
      │ ├─ [4] ⚙️ FUNCTION: VMLogLib.extractRegister(uint256,uint8) (NodeID: 260)
      │ │   💬 Args: [sourceRegs, 2]
      │ │   👁️  Def: private
      │ ├─ [4] ⚙️ FUNCTION: VMLogLib.extractRegister(uint256,uint8) (NodeID: 261)
      │ │   💬 Args: [sourceRegs, 3]
      │ │   👁️  Def: private
      │ ├─ [4] ⚙️ FUNCTION: VMLogLib.extractRegister(uint256,uint8) (NodeID: 262)
      │ │   💬 Args: [sourceRegs, 4]
      │ │   👁️  Def: private
      │ ├─ [4] ⚙️ FUNCTION: RegisterFile.getStatic(bytes[],uint256) (NodeID: 263)
      │ │   💬 Args: [registers, reg1.idx()]
      │ │   👁️  Def: internal
      │ │ └─ [5] ⚙️ FUNCTION: RegisterHelpers.idx(uint8) (NodeID: 264)
      │ │     💬 Args: [reg1]
      │ │     👁️  Def: internal
      │ ├─ [4] ⚙️ FUNCTION: RegisterFile.getStatic(bytes[],uint256) (NodeID: 265)
      │ │   💬 Args: [registers, reg2.idx()]
      │ │   👁️  Def: internal
      │ │ └─ [5] ⚙️ FUNCTION: RegisterHelpers.idx(uint8) (NodeID: 266)
      │ │     💬 Args: [reg2]
      │ │     👁️  Def: internal
      │ ├─ [4] ⚙️ FUNCTION: RegisterFile.getStatic(bytes[],uint256) (NodeID: 267)
      │ │   💬 Args: [registers, reg3.idx()]
      │ │   👁️  Def: internal
      │ │ └─ [5] ⚙️ FUNCTION: RegisterHelpers.idx(uint8) (NodeID: 268)
      │ │     💬 Args: [reg3]
      │ │     👁️  Def: internal
      │ ├─ [4] ⚙️ FUNCTION: RegisterFile.getStatic(bytes[],uint256) (NodeID: 269)
      │ │   💬 Args: [registers, reg4.idx()]
      │ │   👁️  Def: internal
      │ │ └─ [5] ⚙️ FUNCTION: RegisterHelpers.idx(uint8) (NodeID: 270)
      │ │     💬 Args: [reg4]
      │ │     👁️  Def: internal
      │ └─ [4] ⚙️ FUNCTION: RegisterFile.getStatic(bytes[],uint256) (NodeID: 271)
      │     💬 Args: [registers, reg5.idx()]
      │     👁️  Def: internal
      │   └─ [5] ⚙️ FUNCTION: RegisterHelpers.idx(uint8) (NodeID: 272)
      │       💬 Args: [reg5]
      │       👁️  Def: internal
      └─ [3] ⚙️ FUNCTION: VMLogLib.executeDynamic(bytes[],uint256) (NodeID: 273)
          💬 Args: [vmState.registers, cmd.sourceRegs]
          👁️  Def: private
        ├─ [4] ⚙️ FUNCTION: VMLogLib.extractRegister(uint256,uint8) (NodeID: 274)
        │   💬 Args: [sourceRegs, 0]
        │   👁️  Def: private
        └─ [4] ⚙️ FUNCTION: RegisterFile.get(bytes[],uint256) (NodeID: 275)
            💬 Args: [registers, reg.idx()]
            👁️  Def: internal
          └─ [5] ⚙️ FUNCTION: RegisterHelpers.idx(uint8) (NodeID: 276)
              💬 Args: [reg]
              👁️  Def: internal
```

## Documentation

### Function Documentation

@notice Executes commands with state introspection capabilities.
 @dev Returns both the final state snapshot and execution output for debugging/analysis.
