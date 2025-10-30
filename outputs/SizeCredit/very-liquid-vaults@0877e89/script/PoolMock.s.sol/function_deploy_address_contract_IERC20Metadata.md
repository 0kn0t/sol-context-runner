# Function: deploy(address,contract IERC20Metadata)

**Contract**: [script/PoolMock.s.sol/contract_PoolMockScript.md]

## Metadata

- **Contract**: PoolMockScript
- **Signature**: `deploy(address,contract IERC20Metadata)`
- **Visibility**: public
- **Source Range**: 781:242:135

## Implementation

```solidity
function deploy(address owner_, IERC20Metadata asset_) public returns (PoolMock pool) {
    pool = new PoolMock(owner_);
    vm.prank(owner_);
    pool.setLiquidityIndex(address(asset_), WadRayMath.RAY);
    return pool;
}
```

## External Calls

- **Vm::prank(address)**
- **PoolMock::setLiquidityIndex(address,uint256)**

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: PoolMockScript.deploy(address,contract IERC20Metadata) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
```
