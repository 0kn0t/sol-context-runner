# Function: previewSellAssetNoFee(uint256)

**Contract**: [src/EbtcBSM.sol/contract_EbtcBSM.md]

## Metadata

- **Contract**: EbtcBSM
- **Signature**: `previewSellAssetNoFee(uint256)`
- **Visibility**: external
- **Source Range**: 12850:190:40

## Implementation

```solidity
///  @notice Calculates the amount of eBTC minted for a given amount of asset tokens accounting
///  for all minting constraints (no fee)
///  @param _assetAmountIn the total amount intended to be deposited
///  @return _ebtcAmountOut the estimated eBTC to mint after fees
function previewSellAssetNoFee(uint256 _assetAmountIn) external view whenNotPaused() returns (uint256 _ebtcAmountOut) {
    return _previewSellAsset(_assetAmountIn, 0);
}
```

## Related Implementations

### _previewSellAsset(uint256,uint256)

- **Kind**: internal
- **Source**: 5464:406:40
- **Link**: `src/EbtcBSM.sol:EbtcBSM:_previewSellAsset(uint256,uint256)`

```solidity
function _previewSellAsset(uint256 _assetAmountIn, uint256 _feeAmount) private view returns (uint256 _ebtcAmountOut) {
    if (_assetAmountIn == 0) revert ZeroAmount();
    /// @dev _assetAmountIn and _feeAmount are both in asset precision
    _ebtcAmountOut = _toEbtcPrecision(_assetAmountIn - _feeAmount);
    _checkMintingConstraints(_ebtcAmountOut);
}
```

### _toEbtcPrecision(uint256)

- **Kind**: internal
- **Source**: 5322:136:40
- **Link**: `src/EbtcBSM.sol:EbtcBSM:_toEbtcPrecision(uint256)`

```solidity
function _toEbtcPrecision(uint256 _amount) private view returns (uint256) {
    return (_amount * 1e18) / ASSET_TOKEN_PRECISION;
}
```

### _checkMintingConstraints(uint256)

- **Kind**: internal
- **Source**: 7466:793:40
- **Link**: `src/EbtcBSM.sol:EbtcBSM:_checkMintingConstraints(uint256)`

```solidity
/// @notice Internal function to handle the minting constraints checks
///  @param _amountToMint Amount to be minted
function _checkMintingConstraints(uint256 _amountToMint) private view {
    bool success;
    bytes memory errData;
    (success, errData) = oraclePriceConstraint.canProcess(_amountToMint, address(this));
    if (!success) {
        revert IConstraint.ConstraintCheckFailed(address(oraclePriceConstraint), _amountToMint, address(this), errData);
    }
    (success, errData) = rateLimitingConstraint.canProcess(_amountToMint, address(this));
    if (!success) {
        revert IConstraint.ConstraintCheckFailed(address(rateLimitingConstraint), _amountToMint, address(this), errData);
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

- **ASSET_TOKEN_PRECISION** (`uint256`)
- **oraclePriceConstraint** (`contract IConstraint`) [src/Dependencies/IConstraint.sol/interface_IConstraint.md]
- **rateLimitingConstraint** (`contract IConstraint`) [src/Dependencies/IConstraint.sol/interface_IConstraint.md]
- **_paused** (`bool`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: EbtcBSM.previewSellAssetNoFee(uint256) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: external
  ├─ [1] ⚙️ FUNCTION: EbtcBSM._previewSellAsset(uint256,uint256) (NodeID: 1)
  │   💬 Args: [_assetAmountIn, 0]
  │   👁️  Def: private
  │ ├─ [2] ⚙️ FUNCTION: EbtcBSM._toEbtcPrecision(uint256) (NodeID: 2)
  │ │   💬 Args: [_assetAmountIn - _feeAmount]
  │ │   👁️  Def: private
  │ └─ [2] ⚙️ FUNCTION: EbtcBSM._checkMintingConstraints(uint256) (NodeID: 3)
  │     💬 Args: [_ebtcAmountOut]
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

 @notice Calculates the amount of eBTC minted for a given amount of asset tokens accounting
 for all minting constraints (no fee)
 @param _assetAmountIn the total amount intended to be deposited
 @return _ebtcAmountOut the estimated eBTC to mint after fees
