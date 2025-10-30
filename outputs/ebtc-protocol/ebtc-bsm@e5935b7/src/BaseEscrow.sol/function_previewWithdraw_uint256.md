# Function: previewWithdraw(uint256)

**Contract**: [src/BaseEscrow.sol/contract_BaseEscrow.md]

## Metadata

- **Contract**: BaseEscrow
- **Signature**: `previewWithdraw(uint256)`
- **Visibility**: external
- **Source Range**: 4604:225:22

## Implementation

```solidity
function previewWithdraw(uint256 _assetAmount) external view returns (uint256) {
    /// @dev derived contracts can override _previewWithdraw to report potential losses
    return _previewWithdraw(_assetAmount);
}
```

## Related Implementations

### _previewWithdraw(uint256)

- **Kind**: internal
- **Source**: 2798:124:22
- **Link**: `src/BaseEscrow.sol:BaseEscrow:_previewWithdraw(uint256)`

```solidity
/// @notice Internal function to preview the withdrawable amount without making any state changes
///  @param _assetAmount The amount of assets queried for withdrawal
///  @return The amount that can be withdrawn
function _previewWithdraw(uint256 _assetAmount) virtual internal view returns (uint256) {
    return _assetAmount;
}
```

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: BaseEscrow.previewWithdraw(uint256) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: external
  └─ [1] ⚙️ FUNCTION: BaseEscrow._previewWithdraw(uint256) (NodeID: 1)
      💬 Args: [_assetAmount]
      👁️  Def: internal
```
