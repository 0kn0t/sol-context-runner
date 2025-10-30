# Function: previewBuyAssetNoFee(uint256)

**Contract**: [src/EbtcBSM.sol/contract_EbtcBSM.md]

## Metadata

- **Contract**: EbtcBSM
- **Signature**: `previewBuyAssetNoFee(uint256)`
- **Visibility**: external
- **Source Range**: 13305:206:40

## Implementation

```solidity
///  @notice Calculates the net asset amount that can be bought with a given amount of eBTC (no fee)
///  @param _ebtcAmountIn the total amount intended to be deposited
///  @return _assetAmountOut the estimated asset to buy after fees
function previewBuyAssetNoFee(uint256 _ebtcAmountIn) external view whenNotPaused() returns (uint256 _assetAmountOut) {
    return _previewBuyAsset(0, _toAssetPrecision(_ebtcAmountIn));
}
```

## Related Implementations

### _previewBuyAsset(uint256,uint256)

- **Kind**: internal
- **Source**: 5876:439:40
- **Link**: `src/EbtcBSM.sol:EbtcBSM:_previewBuyAsset(uint256,uint256)`

```solidity
function _previewBuyAsset(uint256 _feeAmount, uint256 _ebtcAmountInAssetPrecision) private view returns (uint256 _assetAmountOut) {
    if (_ebtcAmountInAssetPrecision == 0) revert ZeroAmount();
    _checkBuyAssetConstraints(_ebtcAmountInAssetPrecision);
    /// @dev feeAmount is already in asset precision
    _assetAmountOut = escrow.previewWithdraw(_ebtcAmountInAssetPrecision) - _feeAmount;
}
```

### _toAssetPrecision(uint256)

- **Kind**: internal
- **Source**: 5178:138:40
- **Link**: `src/EbtcBSM.sol:EbtcBSM:_toAssetPrecision(uint256)`

```solidity
function _toAssetPrecision(uint256 _amount) private view returns (uint256) {
    return (_amount * ASSET_TOKEN_PRECISION) / 1e18;
}
```

### _checkBuyAssetConstraints(uint256)

- **Kind**: internal
- **Source**: 6548:779:40
- **Link**: `src/EbtcBSM.sol:EbtcBSM:_checkBuyAssetConstraints(uint256)`

```solidity
/// @notice This internal function verifies that the escrow has sufficient assets deposited to cover an amount to buy.
///  @param amountToBuy The amount of assets that is intended to be bought (in asset precision)
function _checkBuyAssetConstraints(uint256 amountToBuy) private view {
    /// @dev totalAssetsDeposited is in asset precision
    uint256 totalAssetsDeposited = escrow.totalAssetsDeposited();
    if (amountToBuy > totalAssetsDeposited) {
        revert InsufficientAssetTokens(amountToBuy, totalAssetsDeposited);
    }
    bool success;
    bytes memory errData;
    (success, errData) = buyAssetConstraint.canProcess(amountToBuy, address(this));
    if (!success) {
        revert IConstraint.ConstraintCheckFailed(address(buyAssetConstraint), amountToBuy, address(this), errData);
    }
}
```

### whenNotPaused()

- **Kind**: modifier
- **Source**: 1439:72:15
- **Link**: `lib/openzeppelin-contracts/contracts/utils/Pausable.sol:Pausable:whenNotPaused()`

```solidity
///  @dev Modifier to make a function callable only when the contract is not paused.
///  Requirements:
///  - The contract must not be paused.
modifier whenNotPaused() {
    _requireNotPaused();
    _;
}
```

### _requireNotPaused()

- **Kind**: internal
- **Source**: 2002:128:15
- **Link**: `lib/openzeppelin-contracts/contracts/utils/Pausable.sol:Pausable:_requireNotPaused()`

```solidity
///  @dev Throws if the contract is paused.
function _requireNotPaused() virtual internal view {
    if (paused()) {
        revert EnforcedPause();
    }
}
```

### paused()

- **Kind**: internal
- **Source**: 1850:84:15
- **Link**: `lib/openzeppelin-contracts/contracts/utils/Pausable.sol:Pausable:paused()`

```solidity
///  @dev Returns true if the contract is paused, and false otherwise.
function paused() virtual public view returns (bool) {
    return _paused;
}
```

## State Variable Reads

- **escrow** (`contract IEscrow`) [src/Dependencies/IEscrow.sol/interface_IEscrow.md]
- **ASSET_TOKEN_PRECISION** (`uint256`)
- **buyAssetConstraint** (`contract IConstraint`) [src/Dependencies/IConstraint.sol/interface_IConstraint.md]
- **_paused** (`bool`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: EbtcBSM.previewBuyAssetNoFee(uint256) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: external
  ├─ [1] ⚙️ FUNCTION: EbtcBSM._previewBuyAsset(uint256,uint256) (NodeID: 1)
  │   💬 Args: [0, _toAssetPrecision(_ebtcAmountIn)]
  │   👁️  Def: private
  │ ├─ [2] ⚙️ FUNCTION: EbtcBSM._toAssetPrecision(uint256) (NodeID: 3)
  │ │   💬 Args: [_ebtcAmountIn]
  │ │   👁️  Def: private
  │ └─ [2] ⚙️ FUNCTION: EbtcBSM._checkBuyAssetConstraints(uint256) (NodeID: 2)
  │     💬 Args: [_ebtcAmountInAssetPrecision]
  │     👁️  Def: private
  └─ [1] 🔒 MODIFIER: Pausable.whenNotPaused() (NodeID: 4)
      💬 Args: [no args]
    └─ [2] ⚙️ FUNCTION: Pausable._requireNotPaused() (NodeID: 5)
        💬 Args: [no args]
        👁️  Def: internal
      └─ [3] ⚙️ FUNCTION: Pausable.paused() (NodeID: 6)
          💬 Args: [no args]
          👁️  Def: public
```

## Documentation

### Function Documentation

 @notice Calculates the net asset amount that can be bought with a given amount of eBTC (no fee)
 @param _ebtcAmountIn the total amount intended to be deposited
 @return _assetAmountOut the estimated asset to buy after fees
