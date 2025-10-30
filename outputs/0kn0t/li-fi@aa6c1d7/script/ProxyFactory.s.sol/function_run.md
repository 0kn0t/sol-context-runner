# Function: run()

**Contract**: [script/ProxyFactory.s.sol/contract_DeployProxyFactory.md]

## Metadata

- **Contract**: DeployProxyFactory
- **Signature**: `run()`
- **Visibility**: external
- **Source Range**: 183:210:19

## Implementation

```solidity
function run() external returns (address) {
    address vmContract = vm.envAddress("VM_CONTRACT");
    bytes32 salt = vm.envBytes32("DEPLOY_SALT");
    return deployWithSalt(salt, vmContract);
}
```

## Related Implementations

### deployWithSalt(bytes32,address)

- **Kind**: internal
- **Source**: 403:1152:19
- **Link**: `script/ProxyFactory.s.sol:DeployProxyFactory:deployWithSalt(bytes32,address)`

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

- **Vm::envAddress(string)**
- **Vm::envBytes32(string)**

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: DeployProxyFactory.run() (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: external
  └─ [1] ⚙️ FUNCTION: DeployProxyFactory.deployWithSalt(bytes32,address) (NodeID: 1)
      💬 Args: [salt, vmContract]
      👁️  Def: public
    ├─ [2] ⚙️ FUNCTION: console.log(string,string) (NodeID: 2)
    │   💬 Args: ["Expected ProxyFactory address with salt:", vm.toString(salt)]
    │   👁️  Def: internal
    │ └─ [3] ⚙️ FUNCTION: StdUtils._sendLogPayload(bytes) (NodeID: 3)
    │     💬 Args: [abi.encodeWithSignature("log(string,string)", p0, p1)]
    │     👁️  Def: internal
    │   └─ [4] ⚙️ FUNCTION: StdUtils._castLogPayloadViewToPure(function (bytes) view) (NodeID: 4)
    │       💬 Args: [_sendLogPayloadView]
    │       👁️  Def: internal
    ├─ [2] ⚙️ FUNCTION: console.logAddress(address) (NodeID: 5)
    │   💬 Args: [predictedAddress]
    │   👁️  Def: internal
    │ └─ [3] ⚙️ FUNCTION: StdUtils._sendLogPayload(bytes) (NodeID: 6)
    │     💬 Args: [abi.encodeWithSignature("log(address)", p0)]
    │     👁️  Def: internal
    │   └─ [4] ⚙️ FUNCTION: StdUtils._castLogPayloadViewToPure(function (bytes) view) (NodeID: 7)
    │       💬 Args: [_sendLogPayloadView]
    │       👁️  Def: internal
    ├─ [2] ⚙️ FUNCTION: console.log(string) (NodeID: 8)
    │   💬 Args: ["ProxyFactory already deployed at expected address"]
    │   👁️  Def: internal
    │ └─ [3] ⚙️ FUNCTION: StdUtils._sendLogPayload(bytes) (NodeID: 9)
    │     💬 Args: [abi.encodeWithSignature("log(string)", p0)]
    │     👁️  Def: internal
    │   └─ [4] ⚙️ FUNCTION: StdUtils._castLogPayloadViewToPure(function (bytes) view) (NodeID: 10)
    │       💬 Args: [_sendLogPayloadView]
    │       👁️  Def: internal
    ├─ [2] ⚙️ FUNCTION: console.log(string,string) (NodeID: 11)
    │   💬 Args: ["ProxyFactory deployed with salt:", vm.toString(salt)]
    │   👁️  Def: internal
    │ └─ [3] ⚙️ FUNCTION: StdUtils._sendLogPayload(bytes) (NodeID: 12)
    │     💬 Args: [abi.encodeWithSignature("log(string,string)", p0, p1)]
    │     👁️  Def: internal
    │   └─ [4] ⚙️ FUNCTION: StdUtils._castLogPayloadViewToPure(function (bytes) view) (NodeID: 13)
    │       💬 Args: [_sendLogPayloadView]
    │       👁️  Def: internal
    └─ [2] ⚙️ FUNCTION: console.logAddress(address) (NodeID: 14)
        💬 Args: [factoryAddress]
        👁️  Def: internal
      └─ [3] ⚙️ FUNCTION: StdUtils._sendLogPayload(bytes) (NodeID: 15)
          💬 Args: [abi.encodeWithSignature("log(address)", p0)]
          👁️  Def: internal
        └─ [4] ⚙️ FUNCTION: StdUtils._castLogPayloadViewToPure(function (bytes) view) (NodeID: 16)
            💬 Args: [_sendLogPayloadView]
            👁️  Def: internal
```
