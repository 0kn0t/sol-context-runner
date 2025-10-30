# Function: deploy(bytes32,bytes32,bytes32,bytes32)

**Contract**: [script/DeployAll.s.sol/contract_DeployAll.md]

## Metadata

- **Contract**: DeployAll
- **Signature**: `deploy(bytes32,bytes32,bytes32,bytes32)`
- **Visibility**: public
- **Source Range**: 2012:2271:15

## Implementation

```solidity
function deploy(bytes32 vmSalt, bytes32 proxyFactorySalt, bytes32 invariantCheckerSalt, bytes32 rpnArithmeticSalt) public returns (address vmAddress, address proxyFactoryAddress, address invariantCheckerAddress, address rpnArithmeticAddress) {
    console.log("=== Deploying Virtual Machine ===");
    console.log("VM Salt:", vm.toString(vmSalt));
    DeployVirtualMachine vmScript = new DeployVirtualMachine();
    vmAddress = vmScript.deployWithSalt(vmSalt);
    console.log("Setting VM_CONTRACT to:", vmAddress);
    vm.setEnv("VM_CONTRACT", vm.toString(vmAddress));
    console.log("\n=== Deploying Proxy Factory ===");
    console.log("ProxyFactory Salt:", vm.toString(proxyFactorySalt));
    DeployProxyFactory proxyScript = new DeployProxyFactory();
    proxyFactoryAddress = proxyScript.deployWithSalt(proxyFactorySalt, vmAddress);
    console.log("\n=== Deploying Invariant Checker ===");
    console.log("InvariantChecker Salt:", vm.toString(invariantCheckerSalt));
    InvariantCheckerScript invariantScript = new InvariantCheckerScript();
    invariantCheckerAddress = invariantScript.deployWithSalt(invariantCheckerSalt);
    console.log("\n=== Deploying RPN Arithmetic ===");
    console.log("RPNArithmetic Salt:", vm.toString(rpnArithmeticSalt));
    RPNArithmeticScript rpnScript = new RPNArithmeticScript();
    rpnArithmeticAddress = rpnScript.deployWithSalt(rpnArithmeticSalt);
    console.log("\n=== Deployment Summary ===");
    console.log("VirtualMachine deployed at:", vmAddress);
    console.log("ProxyFactory deployed at:", proxyFactoryAddress);
    console.log("InvariantChecker deployed at:", invariantCheckerAddress);
    console.log("ArithmeticProcessor deployed at:", rpnArithmeticAddress);
    console.log("Use these addresses when configuring your application");
}
```

## Related Implementations

### log(string)

- **Kind**: internal
- **Source**: 6191:121:11
- **Link**: `lib/forge-std/src/console.sol:console:log(string)`

```solidity
function log(string memory p0) internal pure {
    _sendLogPayload(abi.encodeWithSignature("log(string)", p0));
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

### log(string,string)

- **Kind**: internal
- **Source**: 7439:150:11
- **Link**: `lib/forge-std/src/console.sol:console:log(string,string)`

```solidity
function log(string memory p0, string memory p1) internal pure {
    _sendLogPayload(abi.encodeWithSignature("log(string,string)", p0, p1));
}
```

### log(string,address)

- **Kind**: internal
- **Source**: 7740:145:11
- **Link**: `lib/forge-std/src/console.sol:console:log(string,address)`

```solidity
function log(string memory p0, address p1) internal pure {
    _sendLogPayload(abi.encodeWithSignature("log(string,address)", p0, p1));
}
```

## External Calls

- **Vm::toString(bytes32)**
- **DeployVirtualMachine::deployWithSalt(bytes32)**
- **Vm::setEnv(string,string)**
- **Vm::toString(address)**
- **DeployProxyFactory::deployWithSalt(bytes32,address)**
- **InvariantCheckerScript::deployWithSalt(bytes32)**
- **RPNArithmeticScript::deployWithSalt(bytes32)**

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: DeployAll.deploy(bytes32,bytes32,bytes32,bytes32) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
  ├─ [1] ⚙️ FUNCTION: console.log(string) (NodeID: 1)
  │   💬 Args: ["=== Deploying Virtual Machine ==="]
  │   👁️  Def: internal
  │ └─ [2] ⚙️ FUNCTION: StdUtils._sendLogPayload(bytes) (NodeID: 2)
  │     💬 Args: [abi.encodeWithSignature("log(string)", p0)]
  │     👁️  Def: internal
  │   └─ [3] ⚙️ FUNCTION: StdUtils._castLogPayloadViewToPure(function (bytes) view) (NodeID: 3)
  │       💬 Args: [_sendLogPayloadView]
  │       👁️  Def: internal
  ├─ [1] ⚙️ FUNCTION: console.log(string,string) (NodeID: 4)
  │   💬 Args: ["VM Salt:", vm.toString(vmSalt)]
  │   👁️  Def: internal
  │ └─ [2] ⚙️ FUNCTION: StdUtils._sendLogPayload(bytes) (NodeID: 5)
  │     💬 Args: [abi.encodeWithSignature("log(string,string)", p0, p1)]
  │     👁️  Def: internal
  │   └─ [3] ⚙️ FUNCTION: StdUtils._castLogPayloadViewToPure(function (bytes) view) (NodeID: 6)
  │       💬 Args: [_sendLogPayloadView]
  │       👁️  Def: internal
  ├─ [1] ⚙️ FUNCTION: console.log(string,address) (NodeID: 7)
  │   💬 Args: ["Setting VM_CONTRACT to:", vmAddress]
  │   👁️  Def: internal
  │ └─ [2] ⚙️ FUNCTION: StdUtils._sendLogPayload(bytes) (NodeID: 8)
  │     💬 Args: [abi.encodeWithSignature("log(string,address)", p0, p1)]
  │     👁️  Def: internal
  │   └─ [3] ⚙️ FUNCTION: StdUtils._castLogPayloadViewToPure(function (bytes) view) (NodeID: 9)
  │       💬 Args: [_sendLogPayloadView]
  │       👁️  Def: internal
  ├─ [1] ⚙️ FUNCTION: console.log(string) (NodeID: 10)
  │   💬 Args: ["\n=== Deploying Proxy Factory ==="]
  │   👁️  Def: internal
  │ └─ [2] ⚙️ FUNCTION: StdUtils._sendLogPayload(bytes) (NodeID: 11)
  │     💬 Args: [abi.encodeWithSignature("log(string)", p0)]
  │     👁️  Def: internal
  │   └─ [3] ⚙️ FUNCTION: StdUtils._castLogPayloadViewToPure(function (bytes) view) (NodeID: 12)
  │       💬 Args: [_sendLogPayloadView]
  │       👁️  Def: internal
  ├─ [1] ⚙️ FUNCTION: console.log(string,string) (NodeID: 13)
  │   💬 Args: ["ProxyFactory Salt:", vm.toString(proxyFactorySalt)]
  │   👁️  Def: internal
  │ └─ [2] ⚙️ FUNCTION: StdUtils._sendLogPayload(bytes) (NodeID: 14)
  │     💬 Args: [abi.encodeWithSignature("log(string,string)", p0, p1)]
  │     👁️  Def: internal
  │   └─ [3] ⚙️ FUNCTION: StdUtils._castLogPayloadViewToPure(function (bytes) view) (NodeID: 15)
  │       💬 Args: [_sendLogPayloadView]
  │       👁️  Def: internal
  ├─ [1] ⚙️ FUNCTION: console.log(string) (NodeID: 16)
  │   💬 Args: ["\n=== Deploying Invariant Checker ==="]
  │   👁️  Def: internal
  │ └─ [2] ⚙️ FUNCTION: StdUtils._sendLogPayload(bytes) (NodeID: 17)
  │     💬 Args: [abi.encodeWithSignature("log(string)", p0)]
  │     👁️  Def: internal
  │   └─ [3] ⚙️ FUNCTION: StdUtils._castLogPayloadViewToPure(function (bytes) view) (NodeID: 18)
  │       💬 Args: [_sendLogPayloadView]
  │       👁️  Def: internal
  ├─ [1] ⚙️ FUNCTION: console.log(string,string) (NodeID: 19)
  │   💬 Args: ["InvariantChecker Salt:", vm.toString(invariantCheckerSalt)]
  │   👁️  Def: internal
  │ └─ [2] ⚙️ FUNCTION: StdUtils._sendLogPayload(bytes) (NodeID: 20)
  │     💬 Args: [abi.encodeWithSignature("log(string,string)", p0, p1)]
  │     👁️  Def: internal
  │   └─ [3] ⚙️ FUNCTION: StdUtils._castLogPayloadViewToPure(function (bytes) view) (NodeID: 21)
  │       💬 Args: [_sendLogPayloadView]
  │       👁️  Def: internal
  ├─ [1] ⚙️ FUNCTION: console.log(string) (NodeID: 22)
  │   💬 Args: ["\n=== Deploying RPN Arithmetic ==="]
  │   👁️  Def: internal
  │ └─ [2] ⚙️ FUNCTION: StdUtils._sendLogPayload(bytes) (NodeID: 23)
  │     💬 Args: [abi.encodeWithSignature("log(string)", p0)]
  │     👁️  Def: internal
  │   └─ [3] ⚙️ FUNCTION: StdUtils._castLogPayloadViewToPure(function (bytes) view) (NodeID: 24)
  │       💬 Args: [_sendLogPayloadView]
  │       👁️  Def: internal
  ├─ [1] ⚙️ FUNCTION: console.log(string,string) (NodeID: 25)
  │   💬 Args: ["RPNArithmetic Salt:", vm.toString(rpnArithmeticSalt)]
  │   👁️  Def: internal
  │ └─ [2] ⚙️ FUNCTION: StdUtils._sendLogPayload(bytes) (NodeID: 26)
  │     💬 Args: [abi.encodeWithSignature("log(string,string)", p0, p1)]
  │     👁️  Def: internal
  │   └─ [3] ⚙️ FUNCTION: StdUtils._castLogPayloadViewToPure(function (bytes) view) (NodeID: 27)
  │       💬 Args: [_sendLogPayloadView]
  │       👁️  Def: internal
  ├─ [1] ⚙️ FUNCTION: console.log(string) (NodeID: 28)
  │   💬 Args: ["\n=== Deployment Summary ==="]
  │   👁️  Def: internal
  │ └─ [2] ⚙️ FUNCTION: StdUtils._sendLogPayload(bytes) (NodeID: 29)
  │     💬 Args: [abi.encodeWithSignature("log(string)", p0)]
  │     👁️  Def: internal
  │   └─ [3] ⚙️ FUNCTION: StdUtils._castLogPayloadViewToPure(function (bytes) view) (NodeID: 30)
  │       💬 Args: [_sendLogPayloadView]
  │       👁️  Def: internal
  ├─ [1] ⚙️ FUNCTION: console.log(string,address) (NodeID: 31)
  │   💬 Args: ["VirtualMachine deployed at:", vmAddress]
  │   👁️  Def: internal
  │ └─ [2] ⚙️ FUNCTION: StdUtils._sendLogPayload(bytes) (NodeID: 32)
  │     💬 Args: [abi.encodeWithSignature("log(string,address)", p0, p1)]
  │     👁️  Def: internal
  │   └─ [3] ⚙️ FUNCTION: StdUtils._castLogPayloadViewToPure(function (bytes) view) (NodeID: 33)
  │       💬 Args: [_sendLogPayloadView]
  │       👁️  Def: internal
  ├─ [1] ⚙️ FUNCTION: console.log(string,address) (NodeID: 34)
  │   💬 Args: ["ProxyFactory deployed at:", proxyFactoryAddress]
  │   👁️  Def: internal
  │ └─ [2] ⚙️ FUNCTION: StdUtils._sendLogPayload(bytes) (NodeID: 35)
  │     💬 Args: [abi.encodeWithSignature("log(string,address)", p0, p1)]
  │     👁️  Def: internal
  │   └─ [3] ⚙️ FUNCTION: StdUtils._castLogPayloadViewToPure(function (bytes) view) (NodeID: 36)
  │       💬 Args: [_sendLogPayloadView]
  │       👁️  Def: internal
  ├─ [1] ⚙️ FUNCTION: console.log(string,address) (NodeID: 37)
  │   💬 Args: ["InvariantChecker deployed at:", invariantCheckerAddress]
  │   👁️  Def: internal
  │ └─ [2] ⚙️ FUNCTION: StdUtils._sendLogPayload(bytes) (NodeID: 38)
  │     💬 Args: [abi.encodeWithSignature("log(string,address)", p0, p1)]
  │     👁️  Def: internal
  │   └─ [3] ⚙️ FUNCTION: StdUtils._castLogPayloadViewToPure(function (bytes) view) (NodeID: 39)
  │       💬 Args: [_sendLogPayloadView]
  │       👁️  Def: internal
  ├─ [1] ⚙️ FUNCTION: console.log(string,address) (NodeID: 40)
  │   💬 Args: ["ArithmeticProcessor deployed at:", rpnArithmeticAddress]
  │   👁️  Def: internal
  │ └─ [2] ⚙️ FUNCTION: StdUtils._sendLogPayload(bytes) (NodeID: 41)
  │     💬 Args: [abi.encodeWithSignature("log(string,address)", p0, p1)]
  │     👁️  Def: internal
  │   └─ [3] ⚙️ FUNCTION: StdUtils._castLogPayloadViewToPure(function (bytes) view) (NodeID: 42)
  │       💬 Args: [_sendLogPayloadView]
  │       👁️  Def: internal
  └─ [1] ⚙️ FUNCTION: console.log(string) (NodeID: 43)
      💬 Args: ["Use these addresses when configuring your application"]
      👁️  Def: internal
    └─ [2] ⚙️ FUNCTION: StdUtils._sendLogPayload(bytes) (NodeID: 44)
        💬 Args: [abi.encodeWithSignature("log(string)", p0)]
        👁️  Def: internal
      └─ [3] ⚙️ FUNCTION: StdUtils._castLogPayloadViewToPure(function (bytes) view) (NodeID: 45)
          💬 Args: [_sendLogPayloadView]
          👁️  Def: internal
```
