# Function: run()

**Contract**: [script/InvariantCheckerScript.s.sol/contract_InvariantCheckerScript.md]

## Metadata

- **Contract**: InvariantCheckerScript
- **Signature**: `run()`
- **Visibility**: public
- **Source Range**: 262:137:18

## Implementation

```solidity
function run() public returns (address) {
    bytes32 salt = vm.envBytes32("DEPLOY_SALT");
    return deployWithSalt(salt);
}
```

## Related Implementations

### deployWithSalt(bytes32)

- **Kind**: internal
- **Source**: 409:1065:18
- **Link**: `script/InvariantCheckerScript.s.sol:InvariantCheckerScript:deployWithSalt(bytes32)`

```solidity
function deployWithSalt(bytes32 salt) public returns (address) {
    address expectedAddr = vm.computeCreate2Address(salt, keccak256(abi.encodePacked(type(InvariantChecker).creationCode)));
    console.log("Expected InvariantChecker address with salt:", vm.toString(salt));
    console.logAddress(expectedAddr);
    uint256 codeSize;
    assembly {
        codeSize := extcodesize(expectedAddr)
    }
    if (codeSize > 0) {
        console.log("InvariantChecker already deployed at expected address");
        return expectedAddr;
    }
    vm.startBroadcast();
    InvariantChecker invariantChecker = new InvariantChecker{salt: salt}();
    address addr = address(invariantChecker);
    console.log("InvariantChecker deployed with salt:", vm.toString(salt));
    console.logAddress(addr);
    vm.stopBroadcast();
    return addr;
}
```

### log(string,string)

- **Kind**: internal
- **Source**: 7439:150:11
- **Link**: `lib/forge-std/src/console.sol:console:log(string,string)`

```solidity
function log(string memory p0, string memory p1) internal pure {
    _sendLogPayload(abi.encodeWithSignature("log(string,string)", p0, p1));
}
```

### _sendLogPayload(bytes)

- **Kind**: internal
- **Source**: 9016:133:9
- **Link**: `lib/forge-std/src/StdUtils.sol:StdUtils:_sendLogPayload(bytes)`

```solidity
function _sendLogPayload(bytes memory payload) internal pure {
    _castLogPayloadViewToPure(_sendLogPayloadView)(payload);
}
```

### _castLogPayloadViewToPure(function (bytes)

- **Kind**: internal
- **Source**: 8775:235:9
- **Link**: `lib/forge-std/src/StdUtils.sol:StdUtils:_castLogPayloadViewToPure(function (bytes) view)`

```solidity
function _castLogPayloadViewToPure(function(bytes memory) internal view fnIn) internal pure returns (function(bytes memory) internal pure fnOut) {
    assembly {
        fnOut := fnIn
    }
}
```

### logAddress(address)

- **Kind**: internal
- **Source**: 1589:123:11
- **Link**: `lib/forge-std/src/console.sol:console:logAddress(address)`

```solidity
function logAddress(address p0) internal pure {
    _sendLogPayload(abi.encodeWithSignature("log(address)", p0));
}
```

### log(string)

- **Kind**: internal
- **Source**: 6191:121:11
- **Link**: `lib/forge-std/src/console.sol:console:log(string)`

```solidity
function log(string memory p0) internal pure {
    _sendLogPayload(abi.encodeWithSignature("log(string)", p0));
}
```

## External Calls

- **Vm::envBytes32(string)**

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: InvariantCheckerScript.run() (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
  └─ [1] ⚙️ FUNCTION: InvariantCheckerScript.deployWithSalt(bytes32) (NodeID: 1)
      💬 Args: [salt]
      👁️  Def: public
    ├─ [2] ⚙️ FUNCTION: console.log(string,string) (NodeID: 2)
    │   💬 Args: ["Expected InvariantChecker address with salt:", vm.toString(salt)]
    │   👁️  Def: internal
    │ └─ [3] ⚙️ FUNCTION: StdUtils._sendLogPayload(bytes) (NodeID: 3)
    │     💬 Args: [abi.encodeWithSignature("log(string,string)", p0, p1)]
    │     👁️  Def: internal
    │   └─ [4] ⚙️ FUNCTION: StdUtils._castLogPayloadViewToPure(function (bytes) view) (NodeID: 4)
    │       💬 Args: [_sendLogPayloadView]
    │       👁️  Def: internal
    ├─ [2] ⚙️ FUNCTION: console.logAddress(address) (NodeID: 5)
    │   💬 Args: [expectedAddr]
    │   👁️  Def: internal
    │ └─ [3] ⚙️ FUNCTION: StdUtils._sendLogPayload(bytes) (NodeID: 6)
    │     💬 Args: [abi.encodeWithSignature("log(address)", p0)]
    │     👁️  Def: internal
    │   └─ [4] ⚙️ FUNCTION: StdUtils._castLogPayloadViewToPure(function (bytes) view) (NodeID: 7)
    │       💬 Args: [_sendLogPayloadView]
    │       👁️  Def: internal
    ├─ [2] ⚙️ FUNCTION: console.log(string) (NodeID: 8)
    │   💬 Args: ["InvariantChecker already deployed at expected address"]
    │   👁️  Def: internal
    │ └─ [3] ⚙️ FUNCTION: StdUtils._sendLogPayload(bytes) (NodeID: 9)
    │     💬 Args: [abi.encodeWithSignature("log(string)", p0)]
    │     👁️  Def: internal
    │   └─ [4] ⚙️ FUNCTION: StdUtils._castLogPayloadViewToPure(function (bytes) view) (NodeID: 10)
    │       💬 Args: [_sendLogPayloadView]
    │       👁️  Def: internal
    ├─ [2] ⚙️ FUNCTION: console.log(string,string) (NodeID: 11)
    │   💬 Args: ["InvariantChecker deployed with salt:", vm.toString(salt)]
    │   👁️  Def: internal
    │ └─ [3] ⚙️ FUNCTION: StdUtils._sendLogPayload(bytes) (NodeID: 12)
    │     💬 Args: [abi.encodeWithSignature("log(string,string)", p0, p1)]
    │     👁️  Def: internal
    │   └─ [4] ⚙️ FUNCTION: StdUtils._castLogPayloadViewToPure(function (bytes) view) (NodeID: 13)
    │       💬 Args: [_sendLogPayloadView]
    │       👁️  Def: internal
    └─ [2] ⚙️ FUNCTION: console.logAddress(address) (NodeID: 14)
        💬 Args: [addr]
        👁️  Def: internal
      └─ [3] ⚙️ FUNCTION: StdUtils._sendLogPayload(bytes) (NodeID: 15)
          💬 Args: [abi.encodeWithSignature("log(address)", p0)]
          👁️  Def: internal
        └─ [4] ⚙️ FUNCTION: StdUtils._castLogPayloadViewToPure(function (bytes) view) (NodeID: 16)
            💬 Args: [_sendLogPayloadView]
            👁️  Def: internal
```
