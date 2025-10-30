# Function: run()

**Contract**: [script/Auth.s.sol/contract_AuthScript.md]

## Metadata

- **Contract**: AuthScript
- **Signature**: `run()`
- **Visibility**: public
- **Source Range**: 366:141:124

## Implementation

```solidity
function run() public {
    vm.startBroadcast();
    console.log("Auth", address(deploy(admin)));
    vm.stopBroadcast();
}
```

## Related Implementations

### log(string,address)

- **Kind**: internal
- **Source**: 7740:145:49
- **Link**: `lib/forge-std/src/console.sol:console:log(string,address)`

```solidity
function log(string memory p0, address p1) internal pure {
    _sendLogPayload(abi.encodeWithSignature("log(string,address)", p0, p1));
}
```

### deploy(address)

- **Kind**: internal
- **Source**: 513:178:124
- **Link**: `script/Auth.s.sol:AuthScript:deploy(address)`

```solidity
function deploy(address admin_) public returns (Auth auth) {
    return Auth(address(new ERC1967Proxy(address(new Auth()), abi.encodeCall(Auth.initialize, (admin_)))));
}
```

### _sendLogPayload(bytes)

- **Kind**: internal
- **Source**: 9016:133:47
- **Link**: `lib/forge-std/src/StdUtils.sol:StdUtils:_sendLogPayload(bytes)`

```solidity
function _sendLogPayload(bytes memory payload) internal pure {
    _castLogPayloadViewToPure(_sendLogPayloadView)(payload);
}
```

### _castLogPayloadViewToPure(function (bytes)

- **Kind**: internal
- **Source**: 8775:235:47
- **Link**: `lib/forge-std/src/StdUtils.sol:StdUtils:_castLogPayloadViewToPure(function (bytes) view)`

```solidity
function _castLogPayloadViewToPure(function(bytes memory) internal view fnIn) internal pure returns (function(bytes memory) internal pure fnOut) {
    assembly {
        fnOut := fnIn
    }
}
```

## External Calls

- **Vm::startBroadcast()**
- **Vm::stopBroadcast()**

## State Variable Reads

- **admin** (`address`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: AuthScript.run() (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
  └─ [1] ⚙️ FUNCTION: console.log(string,address) (NodeID: 1)
      💬 Args: ["Auth", address(deploy(admin))]
      👁️  Def: internal
    ├─ [2] ⚙️ FUNCTION: AuthScript.deploy(address) (NodeID: 4)
    │   💬 Args: [admin]
    │   👁️  Def: public
    └─ [2] ⚙️ FUNCTION: StdUtils._sendLogPayload(bytes) (NodeID: 2)
        💬 Args: [abi.encodeWithSignature("log(string,address)", p0, p1)]
        👁️  Def: internal
      └─ [3] ⚙️ FUNCTION: StdUtils._castLogPayloadViewToPure(function (bytes) view) (NodeID: 3)
          💬 Args: [_sendLogPayloadView]
          👁️  Def: internal
```
