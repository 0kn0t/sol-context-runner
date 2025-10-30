# Function: previewSellAsset(uint256)

**Contract**: [src/EbtcBSM.sol/contract_EbtcBSM.md]

## Metadata

- **Contract**: EbtcBSM
- **Signature**: `previewSellAsset(uint256)`
- **Visibility**: external
- **Source Range**: 11770:210:40

## Implementation

```solidity
///  @notice Calculates the amount of eBTC minted for a given amount of asset tokens accounting
///  for all minting constraints
///  @param _assetAmountIn the total amount intended to be deposited
///  @return _ebtcAmountOut the estimated eBTC to mint after fees
function previewSellAsset(uint256 _assetAmountIn) external view whenNotPaused() returns (uint256 _ebtcAmountOut) {
    return _previewSellAsset(_assetAmountIn, _feeToSell(_assetAmountIn));
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

### _feeToSell(uint256)

- **Kind**: internal
- **Source**: 4988:184:40
- **Link**: `src/EbtcBSM.sol:EbtcBSM:_feeToSell(uint256)`

```solidity
/// @notice Calculates the fee for selling asset tokens
///  @param _amount Amount of asset tokens to sell
///  @return Fee amount
function _feeToSell(uint256 _amount) private view returns (uint256) {
    uint256 fee = feeToSellBPS;
    return Math.mulDiv(_amount, fee, fee + BPS, Math.Rounding.Ceil);
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

- **feeToSellBPS** (`uint256`)
- **BPS** (`uint256`)
- **ASSET_TOKEN_PRECISION** (`uint256`)
- **oraclePriceConstraint** (`contract IConstraint`) [src/Dependencies/IConstraint.sol/interface_IConstraint.md]
- **rateLimitingConstraint** (`contract IConstraint`) [src/Dependencies/IConstraint.sol/interface_IConstraint.md]
- **_paused** (`bool`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: EbtcBSM.previewSellAsset(uint256) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: external
  ├─ [1] ⚙️ FUNCTION: EbtcBSM._previewSellAsset(uint256,uint256) (NodeID: 1)
  │   💬 Args: [_assetAmountIn, _feeToSell(_assetAmountIn)]
  │   👁️  Def: private
  │ ├─ [2] ⚙️ FUNCTION: EbtcBSM._feeToSell(uint256) (NodeID: 4)
  │ │   💬 Args: [_assetAmountIn]
  │ │   👁️  Def: private
  │ │ └─ [3] ⚙️ FUNCTION: Math.mulDiv(uint256,uint256,uint256,enum Math.Rounding) (NodeID: 5)
  │ │     💬 Args: [_amount, fee, fee + BPS, Math.Rounding.Ceil]
  │ │     👁️  Def: internal
  │ │   ├─ [4] ⚙️ FUNCTION: SafeCast.toUint(bool) (NodeID: 6)
  │ │   │   💬 Args: [unsignedRoundsUp(rounding) && (mulmod(x, y, denominator) > 0)]
  │ │   │   👁️  Def: internal
  │ │   │ └─ [5] ⚙️ FUNCTION: Math.unsignedRoundsUp(enum Math.Rounding) (NodeID: 7)
  │ │   │     💬 Args: [rounding]
  │ │   │     👁️  Def: internal
  │ │   └─ [4] ⚙️ FUNCTION: Math.mulDiv(uint256,uint256,uint256) (NodeID: 8)
  │ │       💬 Args: [x, y, denominator]
  │ │       👁️  Def: internal
  │ │     └─ [5] ⚙️ FUNCTION: Panic.panic(uint256) (NodeID: 9)
  │ │         💬 Args: [ternary(denominator == 0, Panic.DIVISION_BY_ZERO, Panic.UNDER_OVERFLOW)]
  │ │         👁️  Def: internal
  │ │       └─ [6] ⚙️ FUNCTION: Math.ternary(bool,uint256,uint256) (NodeID: 10)
  │ │           💬 Args: [denominator == 0, Panic.DIVISION_BY_ZERO, Panic.UNDER_OVERFLOW]
  │ │           👁️  Def: internal
  │ │         └─ [7] ⚙️ FUNCTION: SafeCast.toUint(bool) (NodeID: 11)
  │ │             💬 Args: [condition]
  │ │             👁️  Def: internal
  │ ├─ [2] ⚙️ FUNCTION: EbtcBSM._toEbtcPrecision(uint256) (NodeID: 2)
  │ │   💬 Args: [_assetAmountIn - _feeAmount]
  │ │   👁️  Def: private
  │ └─ [2] ⚙️ FUNCTION: EbtcBSM._checkMintingConstraints(uint256) (NodeID: 3)
  │     💬 Args: [_ebtcAmountOut]
  │     👁️  Def: private
  └─ [1] 🔒 MODIFIER: Pausable.whenNotPaused() (NodeID: 12)
      💬 Args: [no args]
    └─ [2] ⚙️ FUNCTION: Pausable._requireNotPaused() (NodeID: 13)
        💬 Args: [no args]
        👁️  Def: internal
      └─ [3] ⚙️ FUNCTION: Pausable.paused() (NodeID: 14)
          💬 Args: [no args]
          👁️  Def: public
```

## Documentation

### Function Documentation

 @notice Calculates the amount of eBTC minted for a given amount of asset tokens accounting
 for all minting constraints
 @param _assetAmountIn the total amount intended to be deposited
 @return _ebtcAmountOut the estimated eBTC to mint after fees
