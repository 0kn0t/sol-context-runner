# Function: run()

**Contract**: [script/VaultMock.s.sol/contract_VaultMockScript.md]

## Metadata

- **Contract**: VaultMockScript
- **Signature**: `run()`
- **Visibility**: public
- **Source Range**: 631:167:141

## Implementation

```solidity
function run() public {
    vm.startBroadcast();
    console.log("VaultMock", address(deploy(owner, asset, name, symbol)));
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

### deploy(address,contract IERC20,string,string)

- **Kind**: internal
- **Source**: 804:207:141
- **Link**: `script/VaultMock.s.sol:VaultMockScript:deploy(address,contract IERC20,string,string)`

```solidity
function deploy(address owner_, IERC20 asset_, string memory name_, string memory symbol_) public returns (VaultMock) {
    return new VaultMock(owner_, asset_, name_, symbol_);
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

- **owner** (`address`)
- **asset** (`contract IERC20`) [lib/openzeppelin-contracts/contracts/token/ERC20/IERC20.sol/interface_IERC20.md]
- **name** (`string`)
- **symbol** (`string`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: VaultMockScript.run() (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
  └─ [1] ⚙️ FUNCTION: console.log(string,address) (NodeID: 1)
      💬 Args: ["VaultMock", address(deploy(owner, asset, name, symbol))]
      👁️  Def: internal
    ├─ [2] ⚙️ FUNCTION: VaultMockScript.deploy(address,contract IERC20,string,string) (NodeID: 4)
    │   💬 Args: [owner, asset, name, symbol]
    │   👁️  Def: public
    └─ [2] ⚙️ FUNCTION: StdUtils._sendLogPayload(bytes) (NodeID: 2)
        💬 Args: [abi.encodeWithSignature("log(string,address)", p0, p1)]
        👁️  Def: internal
      └─ [3] ⚙️ FUNCTION: StdUtils._castLogPayloadViewToPure(function (bytes) view) (NodeID: 3)
          💬 Args: [_sendLogPayloadView]
          👁️  Def: internal
```
