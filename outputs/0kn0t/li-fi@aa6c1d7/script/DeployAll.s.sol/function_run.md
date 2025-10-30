# Function: run()

**Contract**: [script/DeployAll.s.sol/contract_DeployAll.md]

## Metadata

- **Contract**: DeployAll
- **Signature**: `run()`
- **Visibility**: public
- **Source Range**: 734:1268:15

## Implementation

```solidity
function run() public {
    bytes32 salt = vm.envOr("DEPLOY_SALT", bytes32(0));
    bytes32 vmSalt = vm.envOr("VM_SALT", bytes32(0));
    bytes32 proxyFactorySalt = vm.envOr("PROXY_FACTORY_SALT", bytes32(0));
    bytes32 invariantCheckerSalt = vm.envOr("INVARIANT_CHECKER_SALT", bytes32(0));
    bytes32 rpnArithmeticSalt = vm.envOr("RPN_ARITHMETIC_SALT", bytes32(0));
    if (vmSalt == bytes32(0)) {
        vmSalt = (salt != bytes32(0)) ? salt : DEFAULT_VM_SALT;
    }
    if (proxyFactorySalt == bytes32(0)) {
        proxyFactorySalt = (salt != bytes32(0)) ? salt : DEFAULT_PROXY_FACTORY_SALT;
    }
    if (invariantCheckerSalt == bytes32(0)) {
        invariantCheckerSalt = (salt != bytes32(0)) ? salt : DEFAULT_INVARIANT_CHECKER_SALT;
    }
    if (rpnArithmeticSalt == bytes32(0)) {
        rpnArithmeticSalt = (salt != bytes32(0)) ? salt : DEFAULT_RPN_ARITHMETIC_SALT;
    }
    deploy(vmSalt, proxyFactorySalt, invariantCheckerSalt, rpnArithmeticSalt);
}
```

## Related Implementations

### deploy(bytes32,bytes32,bytes32,bytes32)

- **Kind**: internal
- **Source**: 2012:2271:15
- **Link**: `script/DeployAll.s.sol:DeployAll:deploy(bytes32,bytes32,bytes32,bytes32)`

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

- **Vm::envOr(string,bytes32)**

## State Variable Reads

- **DEFAULT_VM_SALT** (`bytes32`)
- **DEFAULT_PROXY_FACTORY_SALT** (`bytes32`)
- **DEFAULT_INVARIANT_CHECKER_SALT** (`bytes32`)
- **DEFAULT_RPN_ARITHMETIC_SALT** (`bytes32`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: DeployAll.run() (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
  └─ [1] ⚙️ FUNCTION: DeployAll.deploy(bytes32,bytes32,bytes32,bytes32) (NodeID: 1)
      💬 Args: [vmSalt, proxyFactorySalt, invariantCheckerSalt, rpnArithmeticSalt]
      👁️  Def: public
    ├─ [2] ⚙️ FUNCTION: console.log(string) (NodeID: 2)
    │   💬 Args: ["=== Deploying Virtual Machine ==="]
    │   👁️  Def: internal
    │ └─ [3] ⚙️ FUNCTION: StdUtils._sendLogPayload(bytes) (NodeID: 3)
    │     💬 Args: [abi.encodeWithSignature("log(string)", p0)]
    │     👁️  Def: internal
    │   └─ [4] ⚙️ FUNCTION: StdUtils._castLogPayloadViewToPure(function (bytes) view) (NodeID: 4)
    │       💬 Args: [_sendLogPayloadView]
    │       👁️  Def: internal
    ├─ [2] ⚙️ FUNCTION: console.log(string,string) (NodeID: 5)
    │   💬 Args: ["VM Salt:", vm.toString(vmSalt)]
    │   👁️  Def: internal
    │ └─ [3] ⚙️ FUNCTION: StdUtils._sendLogPayload(bytes) (NodeID: 6)
    │     💬 Args: [abi.encodeWithSignature("log(string,string)", p0, p1)]
    │     👁️  Def: internal
    │   └─ [4] ⚙️ FUNCTION: StdUtils._castLogPayloadViewToPure(function (bytes) view) (NodeID: 7)
    │       💬 Args: [_sendLogPayloadView]
    │       👁️  Def: internal
    ├─ [2] ⚙️ FUNCTION: console.log(string,address) (NodeID: 8)
    │   💬 Args: ["Setting VM_CONTRACT to:", vmAddress]
    │   👁️  Def: internal
    │ └─ [3] ⚙️ FUNCTION: StdUtils._sendLogPayload(bytes) (NodeID: 9)
    │     💬 Args: [abi.encodeWithSignature("log(string,address)", p0, p1)]
    │     👁️  Def: internal
    │   └─ [4] ⚙️ FUNCTION: StdUtils._castLogPayloadViewToPure(function (bytes) view) (NodeID: 10)
    │       💬 Args: [_sendLogPayloadView]
    │       👁️  Def: internal
    ├─ [2] ⚙️ FUNCTION: console.log(string) (NodeID: 11)
    │   💬 Args: ["\n=== Deploying Proxy Factory ==="]
    │   👁️  Def: internal
    │ └─ [3] ⚙️ FUNCTION: StdUtils._sendLogPayload(bytes) (NodeID: 12)
    │     💬 Args: [abi.encodeWithSignature("log(string)", p0)]
    │     👁️  Def: internal
    │   └─ [4] ⚙️ FUNCTION: StdUtils._castLogPayloadViewToPure(function (bytes) view) (NodeID: 13)
    │       💬 Args: [_sendLogPayloadView]
    │       👁️  Def: internal
    ├─ [2] ⚙️ FUNCTION: console.log(string,string) (NodeID: 14)
    │   💬 Args: ["ProxyFactory Salt:", vm.toString(proxyFactorySalt)]
    │   👁️  Def: internal
    │ └─ [3] ⚙️ FUNCTION: StdUtils._sendLogPayload(bytes) (NodeID: 15)
    │     💬 Args: [abi.encodeWithSignature("log(string,string)", p0, p1)]
    │     👁️  Def: internal
    │   └─ [4] ⚙️ FUNCTION: StdUtils._castLogPayloadViewToPure(function (bytes) view) (NodeID: 16)
    │       💬 Args: [_sendLogPayloadView]
    │       👁️  Def: internal
    ├─ [2] ⚙️ FUNCTION: console.log(string) (NodeID: 17)
    │   💬 Args: ["\n=== Deploying Invariant Checker ==="]
    │   👁️  Def: internal
    │ └─ [3] ⚙️ FUNCTION: StdUtils._sendLogPayload(bytes) (NodeID: 18)
    │     💬 Args: [abi.encodeWithSignature("log(string)", p0)]
    │     👁️  Def: internal
    │   └─ [4] ⚙️ FUNCTION: StdUtils._castLogPayloadViewToPure(function (bytes) view) (NodeID: 19)
    │       💬 Args: [_sendLogPayloadView]
    │       👁️  Def: internal
    ├─ [2] ⚙️ FUNCTION: console.log(string,string) (NodeID: 20)
    │   💬 Args: ["InvariantChecker Salt:", vm.toString(invariantCheckerSalt)]
    │   👁️  Def: internal
    │ └─ [3] ⚙️ FUNCTION: StdUtils._sendLogPayload(bytes) (NodeID: 21)
    │     💬 Args: [abi.encodeWithSignature("log(string,string)", p0, p1)]
    │     👁️  Def: internal
    │   └─ [4] ⚙️ FUNCTION: StdUtils._castLogPayloadViewToPure(function (bytes) view) (NodeID: 22)
    │       💬 Args: [_sendLogPayloadView]
    │       👁️  Def: internal
    ├─ [2] ⚙️ FUNCTION: console.log(string) (NodeID: 23)
    │   💬 Args: ["\n=== Deploying RPN Arithmetic ==="]
    │   👁️  Def: internal
    │ └─ [3] ⚙️ FUNCTION: StdUtils._sendLogPayload(bytes) (NodeID: 24)
    │     💬 Args: [abi.encodeWithSignature("log(string)", p0)]
    │     👁️  Def: internal
    │   └─ [4] ⚙️ FUNCTION: StdUtils._castLogPayloadViewToPure(function (bytes) view) (NodeID: 25)
    │       💬 Args: [_sendLogPayloadView]
    │       👁️  Def: internal
    ├─ [2] ⚙️ FUNCTION: console.log(string,string) (NodeID: 26)
    │   💬 Args: ["RPNArithmetic Salt:", vm.toString(rpnArithmeticSalt)]
    │   👁️  Def: internal
    │ └─ [3] ⚙️ FUNCTION: StdUtils._sendLogPayload(bytes) (NodeID: 27)
    │     💬 Args: [abi.encodeWithSignature("log(string,string)", p0, p1)]
    │     👁️  Def: internal
    │   └─ [4] ⚙️ FUNCTION: StdUtils._castLogPayloadViewToPure(function (bytes) view) (NodeID: 28)
    │       💬 Args: [_sendLogPayloadView]
    │       👁️  Def: internal
    ├─ [2] ⚙️ FUNCTION: console.log(string) (NodeID: 29)
    │   💬 Args: ["\n=== Deployment Summary ==="]
    │   👁️  Def: internal
    │ └─ [3] ⚙️ FUNCTION: StdUtils._sendLogPayload(bytes) (NodeID: 30)
    │     💬 Args: [abi.encodeWithSignature("log(string)", p0)]
    │     👁️  Def: internal
    │   └─ [4] ⚙️ FUNCTION: StdUtils._castLogPayloadViewToPure(function (bytes) view) (NodeID: 31)
    │       💬 Args: [_sendLogPayloadView]
    │       👁️  Def: internal
    ├─ [2] ⚙️ FUNCTION: console.log(string,address) (NodeID: 32)
    │   💬 Args: ["VirtualMachine deployed at:", vmAddress]
    │   👁️  Def: internal
    │ └─ [3] ⚙️ FUNCTION: StdUtils._sendLogPayload(bytes) (NodeID: 33)
    │     💬 Args: [abi.encodeWithSignature("log(string,address)", p0, p1)]
    │     👁️  Def: internal
    │   └─ [4] ⚙️ FUNCTION: StdUtils._castLogPayloadViewToPure(function (bytes) view) (NodeID: 34)
    │       💬 Args: [_sendLogPayloadView]
    │       👁️  Def: internal
    ├─ [2] ⚙️ FUNCTION: console.log(string,address) (NodeID: 35)
    │   💬 Args: ["ProxyFactory deployed at:", proxyFactoryAddress]
    │   👁️  Def: internal
    │ └─ [3] ⚙️ FUNCTION: StdUtils._sendLogPayload(bytes) (NodeID: 36)
    │     💬 Args: [abi.encodeWithSignature("log(string,address)", p0, p1)]
    │     👁️  Def: internal
    │   └─ [4] ⚙️ FUNCTION: StdUtils._castLogPayloadViewToPure(function (bytes) view) (NodeID: 37)
    │       💬 Args: [_sendLogPayloadView]
    │       👁️  Def: internal
    ├─ [2] ⚙️ FUNCTION: console.log(string,address) (NodeID: 38)
    │   💬 Args: ["InvariantChecker deployed at:", invariantCheckerAddress]
    │   👁️  Def: internal
    │ └─ [3] ⚙️ FUNCTION: StdUtils._sendLogPayload(bytes) (NodeID: 39)
    │     💬 Args: [abi.encodeWithSignature("log(string,address)", p0, p1)]
    │     👁️  Def: internal
    │   └─ [4] ⚙️ FUNCTION: StdUtils._castLogPayloadViewToPure(function (bytes) view) (NodeID: 40)
    │       💬 Args: [_sendLogPayloadView]
    │       👁️  Def: internal
    ├─ [2] ⚙️ FUNCTION: console.log(string,address) (NodeID: 41)
    │   💬 Args: ["ArithmeticProcessor deployed at:", rpnArithmeticAddress]
    │   👁️  Def: internal
    │ └─ [3] ⚙️ FUNCTION: StdUtils._sendLogPayload(bytes) (NodeID: 42)
    │     💬 Args: [abi.encodeWithSignature("log(string,address)", p0, p1)]
    │     👁️  Def: internal
    │   └─ [4] ⚙️ FUNCTION: StdUtils._castLogPayloadViewToPure(function (bytes) view) (NodeID: 43)
    │       💬 Args: [_sendLogPayloadView]
    │       👁️  Def: internal
    └─ [2] ⚙️ FUNCTION: console.log(string) (NodeID: 44)
        💬 Args: ["Use these addresses when configuring your application"]
        👁️  Def: internal
      └─ [3] ⚙️ FUNCTION: StdUtils._sendLogPayload(bytes) (NodeID: 45)
          💬 Args: [abi.encodeWithSignature("log(string)", p0)]
          👁️  Def: internal
        └─ [4] ⚙️ FUNCTION: StdUtils._castLogPayloadViewToPure(function (bytes) view) (NodeID: 46)
            💬 Args: [_sendLogPayloadView]
            👁️  Def: internal
```
