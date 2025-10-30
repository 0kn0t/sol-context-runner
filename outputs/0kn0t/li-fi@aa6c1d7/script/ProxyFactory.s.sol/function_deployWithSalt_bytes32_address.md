# Function: deployWithSalt(bytes32,address)

**Contract**: [script/ProxyFactory.s.sol/contract_DeployProxyFactory.md]

## Metadata

- **Contract**: DeployProxyFactory
- **Signature**: `deployWithSalt(bytes32,address)`
- **Visibility**: public
- **Source Range**: 403:1152:19

## Implementation

```solidity
function deployWithSalt(bytes32 salt, address vmContract) public returns (address) {
    bytes32 initCodeHash = keccak256(abi.encodePacked(type(ProxyFactory).creationCode, abi.encode(vmContract)));
    address predictedAddress = vm.computeCreate2Address(salt, initCodeHash);
    console.log("Expected ProxyFactory address with salt:", vm.toString(salt));
    console.logAddress(predictedAddress);
    uint256 codeSize;
    assembly {
        codeSize := extcodesize(predictedAddress)
    }
    if (codeSize > 0) {
        console.log("ProxyFactory already deployed at expected address");
        return predictedAddress;
    }
    vm.startBroadcast();
    ProxyFactory factory = new ProxyFactory{salt: salt}(vmContract);
    address factoryAddress = address(factory);
    console.log("ProxyFactory deployed with salt:", vm.toString(salt));
    console.logAddress(factoryAddress);
    vm.stopBroadcast();
    return factoryAddress;
}
```

## Related Implementations

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

- **Vm::computeCreate2Address(bytes32,bytes32)**
- **Vm::toString(bytes32)**
- **Vm::startBroadcast()**
- **unknown::unknown**
- **Vm::stopBroadcast()**

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: DeployProxyFactory.deployWithSalt(bytes32,address) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
  ├─ [1] ⚙️ FUNCTION: console.log(string,string) (NodeID: 1)
  │   💬 Args: ["Expected ProxyFactory address with salt:", vm.toString(salt)]
  │   👁️  Def: internal
  │ └─ [2] ⚙️ FUNCTION: StdUtils._sendLogPayload(bytes) (NodeID: 2)
  │     💬 Args: [abi.encodeWithSignature("log(string,string)", p0, p1)]
  │     👁️  Def: internal
  │   └─ [3] ⚙️ FUNCTION: StdUtils._castLogPayloadViewToPure(function (bytes) view) (NodeID: 3)
  │       💬 Args: [_sendLogPayloadView]
  │       👁️  Def: internal
  ├─ [1] ⚙️ FUNCTION: console.logAddress(address) (NodeID: 4)
  │   💬 Args: [predictedAddress]
  │   👁️  Def: internal
  │ └─ [2] ⚙️ FUNCTION: StdUtils._sendLogPayload(bytes) (NodeID: 5)
  │     💬 Args: [abi.encodeWithSignature("log(address)", p0)]
  │     👁️  Def: internal
  │   └─ [3] ⚙️ FUNCTION: StdUtils._castLogPayloadViewToPure(function (bytes) view) (NodeID: 6)
  │       💬 Args: [_sendLogPayloadView]
  │       👁️  Def: internal
  ├─ [1] ⚙️ FUNCTION: console.log(string) (NodeID: 7)
  │   💬 Args: ["ProxyFactory already deployed at expected address"]
  │   👁️  Def: internal
  │ └─ [2] ⚙️ FUNCTION: StdUtils._sendLogPayload(bytes) (NodeID: 8)
  │     💬 Args: [abi.encodeWithSignature("log(string)", p0)]
  │     👁️  Def: internal
  │   └─ [3] ⚙️ FUNCTION: StdUtils._castLogPayloadViewToPure(function (bytes) view) (NodeID: 9)
  │       💬 Args: [_sendLogPayloadView]
  │       👁️  Def: internal
  ├─ [1] ⚙️ FUNCTION: console.log(string,string) (NodeID: 10)
  │   💬 Args: ["ProxyFactory deployed with salt:", vm.toString(salt)]
  │   👁️  Def: internal
  │ └─ [2] ⚙️ FUNCTION: StdUtils._sendLogPayload(bytes) (NodeID: 11)
  │     💬 Args: [abi.encodeWithSignature("log(string,string)", p0, p1)]
  │     👁️  Def: internal
  │   └─ [3] ⚙️ FUNCTION: StdUtils._castLogPayloadViewToPure(function (bytes) view) (NodeID: 12)
  │       💬 Args: [_sendLogPayloadView]
  │       👁️  Def: internal
  └─ [1] ⚙️ FUNCTION: console.logAddress(address) (NodeID: 13)
      💬 Args: [factoryAddress]
      👁️  Def: internal
    └─ [2] ⚙️ FUNCTION: StdUtils._sendLogPayload(bytes) (NodeID: 14)
        💬 Args: [abi.encodeWithSignature("log(address)", p0)]
        👁️  Def: internal
      └─ [3] ⚙️ FUNCTION: StdUtils._castLogPayloadViewToPure(function (bytes) view) (NodeID: 15)
          💬 Args: [_sendLogPayloadView]
          👁️  Def: internal
```
