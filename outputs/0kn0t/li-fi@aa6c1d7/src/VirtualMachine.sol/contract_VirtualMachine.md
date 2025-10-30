# Contract: VirtualMachine

## Metadata

- **Name**: VirtualMachine
- **Type**: Contract
- **Path**: src/VirtualMachine.sol

## Public/External Functions

### runVM(struct VMCommand[],struct VMState)

- **Signature**: `runVM(struct VMCommand[],struct VMState)`
- **Visibility**: public
- **Source Range**: 894:165:35
- **Details**: [function_runVM_struct_VMCommand[]_struct_VMState.md](./function_runVM_struct_VMCommand[]_struct_VMState.md)

**Signature:**
```solidity
/// @notice Executes a command sequence and returns the output from a RETURN opcode.
///  @dev Commands execute sequentially with shared register state until RETURN or completion.
function runVM(VMCommand[] calldata commands, VMState memory initialState) public payable returns (bytes memory);
```

### runWithState(struct VMCommand[],struct VMState)

- **Signature**: `runWithState(struct VMCommand[],struct VMState)`
- **Visibility**: public
- **Source Range**: 1234:314:35
- **Details**: [function_runWithState_struct_VMCommand[]_struct_VMState.md](./function_runWithState_struct_VMCommand[]_struct_VMState.md)

**Signature:**
```solidity
/// @notice Executes commands with state introspection capabilities.
///  @dev Returns both the final state snapshot and execution output for debugging/analysis.
function runWithState(VMCommand[] calldata commands, VMState memory initialState) public payable returns (VMState memory, bytes memory);
```

### fallback()

- **Signature**: `fallback()`
- **Visibility**: external
- **Source Range**: 8301:31:35
- **Details**: [function_fallback.md](./function_fallback.md)

**Signature:**
```solidity
/// @dev Accept direct ether transfers for value call operations
fallback() external payable;
```

### receive()

- **Signature**: `receive()`
- **Visibility**: external
- **Source Range**: 8337:30:35
- **Details**: [function_receive.md](./function_receive.md)

**Signature:**
```solidity
receive() external payable;
```
