# Function: buyAsset(uint256,address,uint256)

**Contract**: [src/EbtcBSM.sol/contract_EbtcBSM.md]

## Metadata

- **Contract**: EbtcBSM
- **Signature**: `buyAsset(uint256,address,uint256)`
- **Visibility**: external
- **Source Range**: 14507:438:40

## Implementation

```solidity
///  @notice Allows users to buy BSM owned asset tokens by burning their eBTC
///  @param _ebtcAmountIn Amount of eBTC tokens to burn
///  @param _recipient custom recipient for the asset
///  @param _minOutAmount minimum asset tokens expected after slippage
///  @return _assetAmountOut Amount of asset tokens sent to user
function buyAsset(uint256 _ebtcAmountIn, address _recipient, uint256 _minOutAmount) external whenNotPaused() returns (uint256 _assetAmountOut) {
    uint256 ebtcAmountInAssetPrecision = _toAssetPrecision(_ebtcAmountIn);
    return _buyAsset(_recipient, _feeToBuy(ebtcAmountInAssetPrecision), ebtcAmountInAssetPrecision, _minOutAmount);
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

### _feeToBuy(uint256)

- **Kind**: internal
- **Source**: 4689:149:40
- **Link**: `src/EbtcBSM.sol:EbtcBSM:_feeToBuy(uint256)`

```solidity
/// @notice Calculates the fee for buying asset tokens
///  @param _amount Amount of asset tokens to buy
///  @return Fee amount
function _feeToBuy(uint256 _amount) private view returns (uint256) {
    return Math.mulDiv(_amount, feeToBuyBPS, BPS, Math.Rounding.Ceil);
}
```

### mulDiv(uint256,uint256,uint256,enum Math.Rounding)

- **Kind**: internal
- **Source**: 9351:238:17
- **Link**: `lib/openzeppelin-contracts/contracts/utils/math/Math.sol:Math:mulDiv(uint256,uint256,uint256,enum Math.Rounding)`

```solidity
///  @dev Calculates x * y / denominator with full precision, following the selected rounding direction.
function mulDiv(uint256 x, uint256 y, uint256 denominator, Rounding rounding) internal pure returns (uint256) {
    return mulDiv(x, y, denominator) + SafeCast.toUint(unsignedRoundsUp(rounding) && (mulmod(x, y, denominator) > 0));
}
```

### toUint(bool)

- **Kind**: internal
- **Source**: 34795:145:18
- **Link**: `lib/openzeppelin-contracts/contracts/utils/math/SafeCast.sol:SafeCast:toUint(bool)`

```solidity
///  @dev Cast a boolean (false or true) to a uint256 (0 or 1) with no jump.
function toUint(bool b) internal pure returns (uint256 u) {
    assembly ("memory-safe") {
        u := iszero(iszero(b))
    }
}
```

### unsignedRoundsUp(enum Math.Rounding)

- **Kind**: internal
- **Source**: 28183:122:17
- **Link**: `lib/openzeppelin-contracts/contracts/utils/math/Math.sol:Math:unsignedRoundsUp(enum Math.Rounding)`

```solidity
///  @dev Returns whether a provided rounding mode is considered rounding up for unsigned integers.
function unsignedRoundsUp(Rounding rounding) internal pure returns (bool) {
    return (uint8(rounding) % 2) == 1;
}
```

### mulDiv(uint256,uint256,uint256)

- **Kind**: internal
- **Source**: 4996:4226:17
- **Link**: `lib/openzeppelin-contracts/contracts/utils/math/Math.sol:Math:mulDiv(uint256,uint256,uint256)`

```solidity
///  @dev Calculates floor(x * y / denominator) with full precision. Throws if result overflows a uint256 or
///  denominator == 0.
///  Original credit to Remco Bloemen under MIT license (https://xn--2-umb.com/21/muldiv) with further edits by
///  Uniswap Labs also under MIT license.
function mulDiv(uint256 x, uint256 y, uint256 denominator) internal pure returns (uint256 result) {
    unchecked {
        uint256 prod0 = x * y;
        uint256 prod1;
        assembly {
            let mm := mulmod(x, y, not(0))
            prod1 := sub(sub(mm, prod0), lt(mm, prod0))
        }
        if (prod1 == 0) {
            return prod0 / denominator;
        }
        if (denominator <= prod1) {
            Panic.panic(ternary(denominator == 0, Panic.DIVISION_BY_ZERO, Panic.UNDER_OVERFLOW));
        }
        uint256 remainder;
        assembly {
            remainder := mulmod(x, y, denominator)
            prod1 := sub(prod1, gt(remainder, prod0))
            prod0 := sub(prod0, remainder)
        }
        uint256 twos = denominator & (0 - denominator);
        assembly {
            denominator := div(denominator, twos)
            prod0 := div(prod0, twos)
            twos := add(div(sub(0, twos), twos), 1)
        }
        prod0 |= prod1 * twos;
        uint256 inverse = (3 * denominator) ^ 2;
        inverse *= 2 - (denominator * inverse);
        inverse *= 2 - (denominator * inverse);
        inverse *= 2 - (denominator * inverse);
        inverse *= 2 - (denominator * inverse);
        inverse *= 2 - (denominator * inverse);
        inverse *= 2 - (denominator * inverse);
        result = prod0 * inverse;
        return result;
    }
}
```

### panic(uint256)

- **Kind**: internal
- **Source**: 1776:194:14
- **Link**: `lib/openzeppelin-contracts/contracts/utils/Panic.sol:Panic:panic(uint256)`

```solidity
/// @dev Reverts with a panic code. Recommended to use with
///  the internal constants with predefined codes.
function panic(uint256 code) internal pure {
    assembly ("memory-safe") {
        mstore(0x00, 0x4e487b71)
        mstore(0x20, code)
        revert(0x1c, 0x24)
    }
}
```

### ternary(bool,uint256,uint256)

- **Kind**: internal
- **Source**: 2825:294:17
- **Link**: `lib/openzeppelin-contracts/contracts/utils/math/Math.sol:Math:ternary(bool,uint256,uint256)`

```solidity
///  @dev Branchless ternary evaluation for `a ? b : c`. Gas costs are constant.
///  IMPORTANT: This function may reduce bytecode size and consume less gas when used standalone.
///  However, the compiler may optimize Solidity ternary operations (i.e. `a ? b : c`) to only compute
///  one branch when needed, making this function more expensive.
function ternary(bool condition, uint256 a, uint256 b) internal pure returns (uint256) {
    unchecked {
        return b ^ ((a ^ b) * SafeCast.toUint(condition));
    }
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
- **feeToBuyBPS** (`uint256`)
- **BPS** (`uint256`)
- **buyAssetConstraint** (`contract IConstraint`) [src/Dependencies/IConstraint.sol/interface_IConstraint.md]
- **_paused** (`bool`)

## State Variable Writes

- **totalMinted** (`uint256`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: EbtcBSM.buyAsset(uint256,address,uint256) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: external
  ├─ [1] ⚙️ FUNCTION: EbtcBSM._toAssetPrecision(uint256) (NodeID: 1)
  │   💬 Args: [_ebtcAmountIn]
  │   👁️  Def: private
  ├─ [1] ⚙️ FUNCTION: EbtcBSM._buyAsset(address,uint256,uint256,uint256) (NodeID: 2)
  │   💬 Args: [_recipient, _feeToBuy(ebtcAmountInAssetPrecision), ebtcAmountInAssetPrecision, _minOutAmount]
  │   👁️  Def: internal
  │ ├─ [2] ⚙️ FUNCTION: EbtcBSM._feeToBuy(uint256) (NodeID: 5)
  │ │   💬 Args: [ebtcAmountInAssetPrecision]
  │ │   👁️  Def: private
  │ │ └─ [3] ⚙️ FUNCTION: Math.mulDiv(uint256,uint256,uint256,enum Math.Rounding) (NodeID: 6)
  │ │     💬 Args: [_amount, feeToBuyBPS, BPS, Math.Rounding.Ceil]
  │ │     👁️  Def: internal
  │ │   ├─ [4] ⚙️ FUNCTION: SafeCast.toUint(bool) (NodeID: 7)
  │ │   │   💬 Args: [unsignedRoundsUp(rounding) && (mulmod(x, y, denominator) > 0)]
  │ │   │   👁️  Def: internal
  │ │   │ └─ [5] ⚙️ FUNCTION: Math.unsignedRoundsUp(enum Math.Rounding) (NodeID: 8)
  │ │   │     💬 Args: [rounding]
  │ │   │     👁️  Def: internal
  │ │   └─ [4] ⚙️ FUNCTION: Math.mulDiv(uint256,uint256,uint256) (NodeID: 9)
  │ │       💬 Args: [x, y, denominator]
  │ │       👁️  Def: internal
  │ │     └─ [5] ⚙️ FUNCTION: Panic.panic(uint256) (NodeID: 10)
  │ │         💬 Args: [ternary(denominator == 0, Panic.DIVISION_BY_ZERO, Panic.UNDER_OVERFLOW)]
  │ │         👁️  Def: internal
  │ │       └─ [6] ⚙️ FUNCTION: Math.ternary(bool,uint256,uint256) (NodeID: 11)
  │ │           💬 Args: [denominator == 0, Panic.DIVISION_BY_ZERO, Panic.UNDER_OVERFLOW]
  │ │           👁️  Def: internal
  │ │         └─ [7] ⚙️ FUNCTION: SafeCast.toUint(bool) (NodeID: 12)
  │ │             💬 Args: [condition]
  │ │             👁️  Def: internal
  │ ├─ [2] ⚙️ FUNCTION: EbtcBSM._checkBuyAssetConstraints(uint256) (NodeID: 3)
  │ │   💬 Args: [_ebtcAmountInAssetPrecision]
  │ │   👁️  Def: private
  │ └─ [2] ⚙️ FUNCTION: EbtcBSM._toEbtcPrecision(uint256) (NodeID: 4)
  │     💬 Args: [_ebtcAmountInAssetPrecision]
  │     👁️  Def: private
  └─ [1] 🔒 MODIFIER: Pausable.whenNotPaused() (NodeID: 13)
      💬 Args: [no args]
    └─ [2] ⚙️ FUNCTION: Pausable._requireNotPaused() (NodeID: 14)
        💬 Args: [no args]
        👁️  Def: internal
      └─ [3] ⚙️ FUNCTION: Pausable.paused() (NodeID: 15)
          💬 Args: [no args]
          👁️  Def: public
```

## Documentation

### Function Documentation

 @notice Allows users to buy BSM owned asset tokens by burning their eBTC
 @param _ebtcAmountIn Amount of eBTC tokens to burn
 @param _recipient custom recipient for the asset
 @param _minOutAmount minimum asset tokens expected after slippage
 @return _assetAmountOut Amount of asset tokens sent to user
