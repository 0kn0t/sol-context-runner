# Function: run()

**Contract**: [script/PoolMock.s.sol/contract_PoolMockScript.md]

## Metadata

- **Contract**: PoolMockScript
- **Signature**: `run()`
- **Visibility**: public
- **Source Range**: 623:152:135

## Implementation

```solidity
function run() public {
    vm.startBroadcast();
    console.log("PoolMock", address(deploy(owner, asset)));
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

### deploy(address,contract IERC20Metadata)

- **Kind**: internal
- **Source**: 781:242:135
- **Link**: `script/PoolMock.s.sol:PoolMockScript:deploy(address,contract IERC20Metadata)`

```solidity
function deploy(address owner_, IERC20Metadata asset_) public returns (PoolMock pool) {
    pool = new PoolMock(owner_);
    vm.prank(owner_);
    pool.setLiquidityIndex(address(asset_), WadRayMath.RAY);
    return pool;
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
- **asset** (`contract IERC20Metadata`) [lib/openzeppelin-contracts/contracts/token/ERC20/extensions/IERC20Metadata.sol/interface_IERC20Metadata.md]

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: PoolMockScript.run() (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
  └─ [1] ⚙️ FUNCTION: console.log(string,address) (NodeID: 1)
      💬 Args: ["PoolMock", address(deploy(owner, asset))]
      👁️  Def: internal
    ├─ [2] ⚙️ FUNCTION: PoolMockScript.deploy(address,contract IERC20Metadata) (NodeID: 4)
    │   💬 Args: [owner, asset]
    │   👁️  Def: public
    └─ [2] ⚙️ FUNCTION: StdUtils._sendLogPayload(bytes) (NodeID: 2)
        💬 Args: [abi.encodeWithSignature("log(string,address)", p0, p1)]
        👁️  Def: internal
      └─ [3] ⚙️ FUNCTION: StdUtils._castLogPayloadViewToPure(function (bytes) view) (NodeID: 3)
          💬 Args: [_sendLogPayloadView]
          👁️  Def: internal
```
