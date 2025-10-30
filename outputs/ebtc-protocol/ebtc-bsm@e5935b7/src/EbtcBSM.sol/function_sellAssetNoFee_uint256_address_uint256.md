# Function: sellAssetNoFee(uint256,address,uint256)

**Contract**: [src/EbtcBSM.sol/contract_EbtcBSM.md]

## Metadata

- **Contract**: EbtcBSM
- **Signature**: `sellAssetNoFee(uint256,address,uint256)`
- **Visibility**: external
- **Source Range**: 15381:270:40

## Implementation

```solidity
///  @notice Allows authorized users to mint eBTC by depositing asset tokens without applying a fee
///  @dev can only be called by authorized users
///  @param _assetAmountIn Amount of asset tokens to deposit
///  @param _recipient custom recipient for the minted eBTC
///  @param _minOutAmount minimum eBTC expected after slippage
///  @return _ebtcAmountOut Amount of eBTC tokens minted to the user
function sellAssetNoFee(uint256 _assetAmountIn, address _recipient, uint256 _minOutAmount) external whenNotPaused() requiresAuth() returns (uint256 _ebtcAmountOut) {
    return _sellAsset(_assetAmountIn, _recipient, 0, _minOutAmount);
}
```

## Related Implementations

### _sellAsset(uint256,address,uint256,uint256)

- **Kind**: internal
- **Source**: 8435:1307:40
- **Link**: `src/EbtcBSM.sol:EbtcBSM:_sellAsset(uint256,address,uint256,uint256)`

```solidity
/// @notice Internal sellAsset function with an expected fee amount
///  @dev _ebtcAmountOut might be zero if feeToBuy > 0 and the _ebtcAmountIn its a small value
function _sellAsset(uint256 _assetAmountIn, address _recipient, uint256 _feeAmount, uint256 _minOutAmount) internal returns (uint256 _ebtcAmountOut) {
    if (_assetAmountIn == 0) revert ZeroAmount();
    if (_recipient == address(0)) revert InvalidAddress();
    uint256 assetAmountInNoFee = _assetAmountIn - _feeAmount;
    _ebtcAmountOut = _toEbtcPrecision(assetAmountInNoFee);
    if (_ebtcAmountOut < _minOutAmount) {
        revert BelowExpectedMinOutAmount(_minOutAmount, _ebtcAmountOut);
    }
    _checkMintingConstraints(_ebtcAmountOut);
    totalMinted += _ebtcAmountOut;
    EBTC_TOKEN.mint(_recipient, _ebtcAmountOut);
    escrow.onDeposit(assetAmountInNoFee);
    ASSET_TOKEN.safeTransferFrom(msg.sender, address(escrow), _assetAmountIn);
    emit AssetSold(_assetAmountIn, _ebtcAmountOut, _feeAmount);
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

- **EBTC_TOKEN** (`contract IEbtcToken`) [src/Dependencies/IEbtcToken.sol/interface_IEbtcToken.md]
- **escrow** (`contract IEscrow`) [src/Dependencies/IEscrow.sol/interface_IEscrow.md]
- **ASSET_TOKEN** (`contract IERC20`) [lib/openzeppelin-contracts/contracts/token/ERC20/IERC20.sol/interface_IERC20.md]
- **ASSET_TOKEN_PRECISION** (`uint256`)
- **oraclePriceConstraint** (`contract IConstraint`) [src/Dependencies/IConstraint.sol/interface_IConstraint.md]
- **rateLimitingConstraint** (`contract IConstraint`) [src/Dependencies/IConstraint.sol/interface_IConstraint.md]
- **_authority** (`contract Authority`) [src/Dependencies/Authority.sol/interface_Authority.md]
- **_paused** (`bool`)

## State Variable Writes

- **totalMinted** (`uint256`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: EbtcBSM.sellAssetNoFee(uint256,address,uint256) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: external
  ├─ [1] ⚙️ FUNCTION: EbtcBSM._sellAsset(uint256,address,uint256,uint256) (NodeID: 1)
  │   💬 Args: [_assetAmountIn, _recipient, 0, _minOutAmount]
  │   👁️  Def: internal
  │ ├─ [2] ⚙️ FUNCTION: EbtcBSM._toEbtcPrecision(uint256) (NodeID: 2)
  │ │   💬 Args: [assetAmountInNoFee]
  │ │   👁️  Def: private
  │ └─ [2] ⚙️ FUNCTION: EbtcBSM._checkMintingConstraints(uint256) (NodeID: 3)
  │     💬 Args: [_ebtcAmountOut]
  │     👁️  Def: private
  ├─ [1] 🔒 MODIFIER: AuthNoOwner.requiresAuth() (NodeID: 4)
  │   💬 Args: [no args]
  │ └─ [2] ⚙️ FUNCTION: AuthNoOwner.isAuthorized(address,bytes4) (NodeID: 5)
  │     💬 Args: [msg.sender, msg.sig]
  │     👁️  Def: internal
  └─ [1] 🔒 MODIFIER: Pausable.whenNotPaused() (NodeID: 6)
      💬 Args: [no args]
    └─ [2] ⚙️ FUNCTION: Pausable._requireNotPaused() (NodeID: 7)
        💬 Args: [no args]
        👁️  Def: internal
      └─ [3] ⚙️ FUNCTION: Pausable.paused() (NodeID: 8)
          💬 Args: [no args]
          👁️  Def: public
```

## Documentation

### Function Documentation

 @notice Allows authorized users to mint eBTC by depositing asset tokens without applying a fee
 @dev can only be called by authorized users
 @param _assetAmountIn Amount of asset tokens to deposit
 @param _recipient custom recipient for the minted eBTC
 @param _minOutAmount minimum eBTC expected after slippage
 @return _ebtcAmountOut Amount of eBTC tokens minted to the user
