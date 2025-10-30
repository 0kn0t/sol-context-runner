# Function: buyAssetNoFee(uint256,address,uint256)

**Contract**: [src/EbtcBSM.sol/contract_EbtcBSM.md]

## Metadata

- **Contract**: EbtcBSM
- **Signature**: `buyAssetNoFee(uint256,address,uint256)`
- **Visibility**: external
- **Source Range**: 16069:359:40

## Implementation

```solidity
///  @notice Allows authorized users to buy BSM owned asset tokens by burning their eBTC
///  @dev Can only be called by authorized users
///  @param _ebtcAmountIn Amount of eBTC tokens to burn
///  @param _recipient custom recipient for the asset
///  @param _minOutAmount minimum asset tokens expected after slippage
///  @return _assetAmountOut Amount of asset tokens sent to user
function buyAssetNoFee(uint256 _ebtcAmountIn, address _recipient, uint256 _minOutAmount) external whenNotPaused() requiresAuth() returns (uint256 _assetAmountOut) {
    uint256 ebtcAmountInAssetPrecision = _toAssetPrecision(_ebtcAmountIn);
    return _buyAsset(_recipient, 0, ebtcAmountInAssetPrecision, _minOutAmount);
}
```

## Related Implementations

### _toAssetPrecision(uint256)

- **Kind**: internal
- **Source**: 5178:138:40
- **Link**: `src/EbtcBSM.sol:EbtcBSM:_toAssetPrecision(uint256)`

```solidity
function _toAssetPrecision(uint256 _amount) private view returns (uint256) {
    return (_amount * ASSET_TOKEN_PRECISION) / 1e18;
}
```

### _buyAsset(address,uint256,uint256,uint256)

- **Kind**: internal
- **Source**: 9956:1519:40
- **Link**: `src/EbtcBSM.sol:EbtcBSM:_buyAsset(address,uint256,uint256,uint256)`

```solidity
/// @notice Internal buyAsset function with an expected fee amount
///  @dev _assetAmountOut might be zero if feeToBuy > 0 and the _ebtcAmountIn its a small value e.g. _ebtcAmountIn = 1e10 && fee = 1%
function _buyAsset(address _recipient, uint256 _feeAmount, uint256 _ebtcAmountInAssetPrecision, uint256 _minOutAmount) internal returns (uint256 _assetAmountOut) {
    if (_ebtcAmountInAssetPrecision == 0) revert ZeroAmount();
    if (_recipient == address(0)) revert InvalidAddress();
    /// @dev ok to pass amount without fee to constraint
    ///  fee amount can be deducted by the constraint if necessary
    _checkBuyAssetConstraints(_ebtcAmountInAssetPrecision);
    /// @dev this prevents burning of eBTC below asset precision
    uint256 ebtcToBurn = _toEbtcPrecision(_ebtcAmountInAssetPrecision);
    totalMinted -= ebtcToBurn;
    EBTC_TOKEN.burn(msg.sender, ebtcToBurn);
    uint256 redeemedAmount = escrow.onWithdraw(_ebtcAmountInAssetPrecision);
    /// @dev _feeAmount is in asset precision
    _assetAmountOut = redeemedAmount - _feeAmount;
    if (_assetAmountOut < _minOutAmount) {
        revert BelowExpectedMinOutAmount(_minOutAmount, _assetAmountOut);
    }
    if (_assetAmountOut > 0) {
        ASSET_TOKEN.safeTransferFrom(address(escrow), _recipient, _assetAmountOut);
    }
    emit AssetBought(ebtcToBurn, _assetAmountOut, _feeAmount);
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

### _toEbtcPrecision(uint256)

- **Kind**: internal
- **Source**: 5322:136:40
- **Link**: `src/EbtcBSM.sol:EbtcBSM:_toEbtcPrecision(uint256)`

```solidity
function _toEbtcPrecision(uint256 _amount) private view returns (uint256) {
    return (_amount * 1e18) / ASSET_TOKEN_PRECISION;
}
```

### requiresAuth()

- **Kind**: modifier
- **Source**: 647:125:25
- **Link**: `src/Dependencies/AuthNoOwner.sol:AuthNoOwner:requiresAuth()`

```solidity
modifier requiresAuth() virtual {
    require(isAuthorized(msg.sender, msg.sig), "Auth: UNAUTHORIZED");
    _;
}
```

### isAuthorized(address,bytes4)

- **Kind**: internal
- **Source**: 981:524:25
- **Link**: `src/Dependencies/AuthNoOwner.sol:AuthNoOwner:isAuthorized(address,bytes4)`

```solidity
function isAuthorized(address user, bytes4 functionSig) virtual internal view returns (bool) {
    Authority auth = _authority;
    return ((address(auth) != address(0)) && auth.canCall(user, address(this), functionSig));
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
- **EBTC_TOKEN** (`contract IEbtcToken`) [src/Dependencies/IEbtcToken.sol/interface_IEbtcToken.md]
- **escrow** (`contract IEscrow`) [src/Dependencies/IEscrow.sol/interface_IEscrow.md]
- **ASSET_TOKEN** (`contract IERC20`) [lib/openzeppelin-contracts/contracts/token/ERC20/IERC20.sol/interface_IERC20.md]
- **buyAssetConstraint** (`contract IConstraint`) [src/Dependencies/IConstraint.sol/interface_IConstraint.md]
- **_authority** (`contract Authority`) [src/Dependencies/Authority.sol/interface_Authority.md]
- **_paused** (`bool`)

## State Variable Writes

- **totalMinted** (`uint256`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: EbtcBSM.buyAssetNoFee(uint256,address,uint256) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: external
  ├─ [1] ⚙️ FUNCTION: EbtcBSM._toAssetPrecision(uint256) (NodeID: 1)
  │   💬 Args: [_ebtcAmountIn]
  │   👁️  Def: private
  ├─ [1] ⚙️ FUNCTION: EbtcBSM._buyAsset(address,uint256,uint256,uint256) (NodeID: 2)
  │   💬 Args: [_recipient, 0, ebtcAmountInAssetPrecision, _minOutAmount]
  │   👁️  Def: internal
  │ ├─ [2] ⚙️ FUNCTION: EbtcBSM._checkBuyAssetConstraints(uint256) (NodeID: 3)
  │ │   💬 Args: [_ebtcAmountInAssetPrecision]
  │ │   👁️  Def: private
  │ └─ [2] ⚙️ FUNCTION: EbtcBSM._toEbtcPrecision(uint256) (NodeID: 4)
  │     💬 Args: [_ebtcAmountInAssetPrecision]
  │     👁️  Def: private
  ├─ [1] 🔒 MODIFIER: AuthNoOwner.requiresAuth() (NodeID: 5)
  │   💬 Args: [no args]
  │ └─ [2] ⚙️ FUNCTION: AuthNoOwner.isAuthorized(address,bytes4) (NodeID: 6)
  │     💬 Args: [msg.sender, msg.sig]
  │     👁️  Def: internal
  └─ [1] 🔒 MODIFIER: Pausable.whenNotPaused() (NodeID: 7)
      💬 Args: [no args]
    └─ [2] ⚙️ FUNCTION: Pausable._requireNotPaused() (NodeID: 8)
        💬 Args: [no args]
        👁️  Def: internal
      └─ [3] ⚙️ FUNCTION: Pausable.paused() (NodeID: 9)
          💬 Args: [no args]
          👁️  Def: public
```

## Documentation

### Function Documentation

 @notice Allows authorized users to buy BSM owned asset tokens by burning their eBTC
 @dev Can only be called by authorized users
 @param _ebtcAmountIn Amount of eBTC tokens to burn
 @param _recipient custom recipient for the asset
 @param _minOutAmount minimum asset tokens expected after slippage
 @return _assetAmountOut Amount of asset tokens sent to user
