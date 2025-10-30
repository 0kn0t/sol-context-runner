# Function: maxMint(address)

**Contract**: [src/SizeMetaVault.sol/contract_SizeMetaVault.md]

## Metadata

- **Contract**: SizeMetaVault
- **Signature**: `maxMint(address)`
- **Visibility**: public
- **Source Range**: 4275:353:59

## Implementation

```solidity
/// @notice Returns the maximum number of shares that can be minted
///  @dev Converts the max deposit amount to shares
function maxMint(address receiver) override(BaseVault) public view returns (uint256) {
    uint256 maxDepositAmount = maxDeposit(receiver);
    uint256 maxMintAmount = (maxDepositAmount == type(uint256).max) ? type(uint256).max : convertToShares(maxDepositAmount);
    return Math.min(maxMintAmount, super.maxMint(receiver));
}
```

## Related Implementations

### maxDeposit(address)

- **Kind**: internal
- **Source**: 3979:163:59
- **Link**: `src/SizeMetaVault.sol:SizeMetaVault:maxDeposit(address)`

```solidity
/// @notice Returns the maximum amount that can be deposited
function maxDeposit(address receiver) override(BaseVault) public view returns (uint256) {
    return Math.min(_maxDeposit(), super.maxDeposit(receiver));
}
```

### min(uint256,uint256)

- **Kind**: internal
- **Source**: 5617:111:52
- **Link**: `lib/openzeppelin-contracts/contracts/utils/math/Math.sol:Math:min(uint256,uint256)`

```solidity
///  @dev Returns the smallest of two numbers.
function min(uint256 a, uint256 b) internal pure returns (uint256) {
    return ternary(a < b, a, b);
}
```

### _maxDeposit()

- **Kind**: internal
- **Source**: 15537:355:59
- **Link**: `src/SizeMetaVault.sol:SizeMetaVault:_maxDeposit()`

```solidity
/// @notice Internal function to calculate maximum depositable amount in all strategies
function _maxDeposit() private view returns (uint256 max) {
    uint256 length = strategies.length;
    for (uint256 i = 0; i < length; i++) {
        IBaseVault strategy = strategies[i];
        uint256 strategyMaxDeposit = strategy.maxDeposit(address(this));
        max = Math.saturatingAdd(max, strategyMaxDeposit);
    }
}
```

### saturatingAdd(uint256,uint256)

- **Kind**: internal
- **Source**: 3912:199:52
- **Link**: `lib/openzeppelin-contracts/contracts/utils/math/Math.sol:Math:saturatingAdd(uint256,uint256)`

```solidity
///  @dev Unsigned saturating addition, bounds to `2²⁵⁶ - 1` instead of overflowing.
function saturatingAdd(uint256 a, uint256 b) internal pure returns (uint256) {
    (bool success, uint256 result) = tryAdd(a, b);
    return ternary(success, result, type(uint256).max);
}
```

### tryAdd(uint256,uint256)

- **Kind**: internal
- **Source**: 1693:240:52
- **Link**: `lib/openzeppelin-contracts/contracts/utils/math/Math.sol:Math:tryAdd(uint256,uint256)`

```solidity
///  @dev Returns the addition of two unsigned integers, with a success flag (no overflow).
function tryAdd(uint256 a, uint256 b) internal pure returns (bool success, uint256 result) {
    unchecked {
        uint256 c = a + b;
        success = c >= a;
        result = c * SafeCast.toUint(success);
    }
}
```

### toUint(bool)

- **Kind**: internal
- **Source**: 34795:145:53
- **Link**: `lib/openzeppelin-contracts/contracts/utils/math/SafeCast.sol:SafeCast:toUint(bool)`

```solidity
///  @dev Cast a boolean (false or true) to a uint256 (0 or 1) with no jump.
function toUint(bool b) internal pure returns (uint256 u) {
    assembly ("memory-safe") {
        u := iszero(iszero(b))
    }
}
```

### ternary(bool,uint256,uint256)

- **Kind**: internal
- **Source**: 5071:294:52
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

### maxDeposit(address)

- **Kind**: internal
- **Source**: 8184:249:56
- **Link**: `src/BaseVault.sol:BaseVault:maxDeposit(address)`

```solidity
/// @notice Returns the maximum amount that can be deposited
///  @dev Returns type(uint256).max if no total assets cap is set
function maxDeposit(address) virtual override(ERC4626Upgradeable, IERC4626) public view returns (uint256) {
    return (totalAssetsCap == type(uint256).max) ? type(uint256).max : Math.saturatingSub(totalAssetsCap, totalAssets());
}
```

### saturatingSub(uint256,uint256)

- **Kind**: internal
- **Source**: 4217:150:52
- **Link**: `lib/openzeppelin-contracts/contracts/utils/math/Math.sol:Math:saturatingSub(uint256,uint256)`

```solidity
///  @dev Unsigned saturating subtraction, bounds to zero instead of overflowing.
function saturatingSub(uint256 a, uint256 b) internal pure returns (uint256) {
    (, uint256 result) = trySub(a, b);
    return result;
}
```

### totalAssets()

- **Kind**: internal
- **Source**: 5471:264:59
- **Link**: `src/SizeMetaVault.sol:SizeMetaVault:totalAssets()`

```solidity
/// @notice Returns the total assets managed by the vault
function totalAssets() virtual override(ERC4626Upgradeable, IERC4626) public view returns (uint256 total) {
    uint256 length = strategies.length;
    for (uint256 i = 0; i < length; i++) {
        total += strategies[i].totalAssets();
    }
}
```

### trySub(uint256,uint256)

- **Kind**: internal
- **Source**: 2052:240:52
- **Link**: `lib/openzeppelin-contracts/contracts/utils/math/Math.sol:Math:trySub(uint256,uint256)`

```solidity
///  @dev Returns the subtraction of two unsigned integers, with a success flag (no overflow).
function trySub(uint256 a, uint256 b) internal pure returns (bool success, uint256 result) {
    unchecked {
        uint256 c = a - b;
        success = c <= a;
        result = c * SafeCast.toUint(success);
    }
}
```

### convertToShares(uint256)

- **Kind**: internal
- **Source**: 7264:148:17
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/token/ERC20/extensions/ERC4626Upgradeable.sol:ERC4626Upgradeable:convertToShares(uint256)`

```solidity
/// @dev See {IERC4626-convertToShares}. 
function convertToShares(uint256 assets) virtual public view returns (uint256) {
    return _convertToShares(assets, Math.Rounding.Floor);
}
```

### _convertToShares(uint256,enum Math.Rounding)

- **Kind**: internal
- **Source**: 10972:213:17
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/token/ERC20/extensions/ERC4626Upgradeable.sol:ERC4626Upgradeable:_convertToShares(uint256,enum Math.Rounding)`

```solidity
///  @dev Internal conversion function (from assets to shares) with support for rounding direction.
function _convertToShares(uint256 assets, Math.Rounding rounding) virtual internal view returns (uint256) {
    return assets.mulDiv(totalSupply() + (10 ** _decimalsOffset()), totalAssets() + 1, rounding);
}
```

### mulDiv(uint256,uint256,uint256,enum Math.Rounding)

- **Kind**: internal
- **Source**: 11054:238:52
- **Link**: `lib/openzeppelin-contracts/contracts/utils/math/Math.sol:Math:mulDiv(uint256,uint256,uint256,enum Math.Rounding)`

```solidity
///  @dev Calculates x * y / denominator with full precision, following the selected rounding direction.
function mulDiv(uint256 x, uint256 y, uint256 denominator, Rounding rounding) internal pure returns (uint256) {
    return mulDiv(x, y, denominator) + SafeCast.toUint(unsignedRoundsUp(rounding) && (mulmod(x, y, denominator) > 0));
}
```

### _decimalsOffset()

- **Kind**: internal
- **Source**: 13425:90:17
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/token/ERC20/extensions/ERC4626Upgradeable.sol:ERC4626Upgradeable:_decimalsOffset()`

```solidity
function _decimalsOffset() virtual internal view returns (uint8) {
    return 0;
}
```

### totalSupply()

- **Kind**: internal
- **Source**: 3877:152:15
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/token/ERC20/ERC20Upgradeable.sol:ERC20Upgradeable:totalSupply()`

```solidity
///  @dev See {IERC20-totalSupply}.
function totalSupply() virtual public view returns (uint256) {
    ERC20Storage storage $ = _getERC20Storage();
    return $._totalSupply;
}
```

### _getERC20Storage()

- **Kind**: internal
- **Source**: 1947:153:15
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/token/ERC20/ERC20Upgradeable.sol:ERC20Upgradeable:_getERC20Storage()`

```solidity
function _getERC20Storage() private pure returns (ERC20Storage storage $) {
    assembly {
        $.slot := ERC20StorageLocation
    }
}
```

### unsignedRoundsUp(enum Math.Rounding)

- **Kind**: internal
- **Source**: 32020:122:52
- **Link**: `lib/openzeppelin-contracts/contracts/utils/math/Math.sol:Math:unsignedRoundsUp(enum Math.Rounding)`

```solidity
///  @dev Returns whether a provided rounding mode is considered rounding up for unsigned integers.
function unsignedRoundsUp(Rounding rounding) internal pure returns (bool) {
    return (uint8(rounding) % 2) == 1;
}
```

### mulDiv(uint256,uint256,uint256)

- **Kind**: internal
- **Source**: 7242:3683:52
- **Link**: `lib/openzeppelin-contracts/contracts/utils/math/Math.sol:Math:mulDiv(uint256,uint256,uint256)`

```solidity
///  @dev Calculates floor(x * y / denominator) with full precision. Throws if result overflows a uint256 or
///  denominator == 0.
///  Original credit to Remco Bloemen under MIT license (https://xn--2-umb.com/21/muldiv) with further edits by
///  Uniswap Labs also under MIT license.
function mulDiv(uint256 x, uint256 y, uint256 denominator) internal pure returns (uint256 result) {
    unchecked {
        (uint256 high, uint256 low) = mul512(x, y);
        if (high == 0) {
            return low / denominator;
        }
        if (denominator <= high) {
            Panic.panic(ternary(denominator == 0, Panic.DIVISION_BY_ZERO, Panic.UNDER_OVERFLOW));
        }
        uint256 remainder;
        assembly ("memory-safe") {
            remainder := mulmod(x, y, denominator)
            high := sub(high, gt(remainder, low))
            low := sub(low, remainder)
        }
        uint256 twos = denominator & (0 - denominator);
        assembly ("memory-safe") {
            denominator := div(denominator, twos)
            low := div(low, twos)
            twos := add(div(sub(0, twos), twos), 1)
        }
        low |= high * twos;
        uint256 inverse = (3 * denominator) ^ 2;
        inverse *= 2 - (denominator * inverse);
        inverse *= 2 - (denominator * inverse);
        inverse *= 2 - (denominator * inverse);
        inverse *= 2 - (denominator * inverse);
        inverse *= 2 - (denominator * inverse);
        inverse *= 2 - (denominator * inverse);
        result = low * inverse;
        return result;
    }
}
```

### mul512(uint256,uint256)

- **Kind**: internal
- **Source**: 1027:550:52
- **Link**: `lib/openzeppelin-contracts/contracts/utils/math/Math.sol:Math:mul512(uint256,uint256)`

```solidity
///  @dev Return the 512-bit multiplication of two uint256.
///  The result is stored in two 256 variables such that product = high * 2²⁵⁶ + low.
function mul512(uint256 a, uint256 b) internal pure returns (uint256 high, uint256 low) {
    assembly ("memory-safe") {
        let mm := mulmod(a, b, not(0))
        low := mul(a, b)
        high := sub(sub(mm, low), lt(mm, low))
    }
}
```

### panic(uint256)

- **Kind**: internal
- **Source**: 1776:194:45
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

### maxMint(address)

- **Kind**: internal
- **Source**: 8570:231:56
- **Link**: `src/BaseVault.sol:BaseVault:maxMint(address)`

```solidity
/// @notice Returns the maximum amount that can be minted
///  @dev Returns type(uint256).max if no total assets cap is set
function maxMint(address receiver) virtual override(ERC4626Upgradeable, IERC4626) public view returns (uint256) {
    return (totalAssetsCap == type(uint256).max) ? type(uint256).max : convertToShares(maxDeposit(receiver));
}
```

## State Variable Reads

- **strategies** (`contract IBaseVault[]`) [src/IBaseVault.sol/interface_IBaseVault.md]
- **totalAssetsCap** (`uint256`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: SizeMetaVault.maxMint(address) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
  ├─ [1] ⚙️ FUNCTION: SizeMetaVault.maxDeposit(address) (NodeID: 1)
  │   💬 Args: [receiver]
  │   👁️  Def: public
  │ └─ [2] ⚙️ FUNCTION: Math.min(uint256,uint256) (NodeID: 2)
  │     💬 Args: [_maxDeposit(), super.maxDeposit(receiver)]
  │     👁️  Def: internal
  │   ├─ [3] ⚙️ FUNCTION: SizeMetaVault._maxDeposit() (NodeID: 5)
  │   │   💬 Args: [no args]
  │   │   👁️  Def: private
  │   │ └─ [4] ⚙️ FUNCTION: Math.saturatingAdd(uint256,uint256) (NodeID: 6)
  │   │     💬 Args: [max, strategyMaxDeposit]
  │   │     👁️  Def: internal
  │   │   ├─ [5] ⚙️ FUNCTION: Math.tryAdd(uint256,uint256) (NodeID: 7)
  │   │   │   💬 Args: [a, b]
  │   │   │   👁️  Def: internal
  │   │   │ └─ [6] ⚙️ FUNCTION: SafeCast.toUint(bool) (NodeID: 8)
  │   │   │     💬 Args: [success]
  │   │   │     👁️  Def: internal
  │   │   └─ [5] ⚙️ FUNCTION: Math.ternary(bool,uint256,uint256) (NodeID: 9)
  │   │       💬 Args: [success, result, type(uint256).max]
  │   │       👁️  Def: internal
  │   │     └─ [6] ⚙️ FUNCTION: SafeCast.toUint(bool) (NodeID: 10)
  │   │         💬 Args: [condition]
  │   │         👁️  Def: internal
  │   ├─ [3] ⚙️ FUNCTION: BaseVault.maxDeposit(address) (NodeID: 11)
  │   │   💬 Args: [receiver]
  │   │   👁️  Def: public
  │   │ └─ [4] ⚙️ FUNCTION: Math.saturatingSub(uint256,uint256) (NodeID: 12)
  │   │     💬 Args: [totalAssetsCap, totalAssets()]
  │   │     👁️  Def: internal
  │   │   ├─ [5] ⚙️ FUNCTION: SizeMetaVault.totalAssets() (NodeID: 15)
  │   │   │   💬 Args: [no args]
  │   │   │   👁️  Def: public
  │   │   └─ [5] ⚙️ FUNCTION: Math.trySub(uint256,uint256) (NodeID: 13)
  │   │       💬 Args: [a, b]
  │   │       👁️  Def: internal
  │   │     └─ [6] ⚙️ FUNCTION: SafeCast.toUint(bool) (NodeID: 14)
  │   │         💬 Args: [success]
  │   │         👁️  Def: internal
  │   └─ [3] ⚙️ FUNCTION: Math.ternary(bool,uint256,uint256) (NodeID: 3)
  │       💬 Args: [a < b, a, b]
  │       👁️  Def: internal
  │     └─ [4] ⚙️ FUNCTION: SafeCast.toUint(bool) (NodeID: 4)
  │         💬 Args: [condition]
  │         👁️  Def: internal
  ├─ [1] ⚙️ FUNCTION: ERC4626Upgradeable.convertToShares(uint256) (NodeID: 16)
  │   💬 Args: [maxDepositAmount]
  │   👁️  Def: public
  │ └─ [2] ⚙️ FUNCTION: ERC4626Upgradeable._convertToShares(uint256,enum Math.Rounding) (NodeID: 17)
  │     💬 Args: [assets, Math.Rounding.Floor]
  │     👁️  Def: internal
  │   └─ [3] ⚙️ FUNCTION: Math.mulDiv(uint256,uint256,uint256,enum Math.Rounding) (NodeID: 18)
  │       💬 Args: [assets, totalSupply() + (10 ** _decimalsOffset()), totalAssets() + 1, rounding]
  │       👁️  Def: internal
  │     ├─ [4] ⚙️ FUNCTION: ERC4626Upgradeable._decimalsOffset() (NodeID: 26)
  │     │   💬 Args: [no args]
  │     │   👁️  Def: internal
  │     ├─ [4] ⚙️ FUNCTION: ERC20Upgradeable.totalSupply() (NodeID: 27)
  │     │   💬 Args: [no args]
  │     │   👁️  Def: public
  │     │ └─ [5] ⚙️ FUNCTION: ERC20Upgradeable._getERC20Storage() (NodeID: 28)
  │     │     💬 Args: [no args]
  │     │     👁️  Def: private
  │     ├─ [4] ⚙️ FUNCTION: SizeMetaVault.totalAssets() (NodeID: 29)
  │     │   💬 Args: [no args]
  │     │   👁️  Def: public
  │     ├─ [4] ⚙️ FUNCTION: SafeCast.toUint(bool) (NodeID: 19)
  │     │   💬 Args: [unsignedRoundsUp(rounding) && (mulmod(x, y, denominator) > 0)]
  │     │   👁️  Def: internal
  │     │ └─ [5] ⚙️ FUNCTION: Math.unsignedRoundsUp(enum Math.Rounding) (NodeID: 20)
  │     │     💬 Args: [rounding]
  │     │     👁️  Def: internal
  │     └─ [4] ⚙️ FUNCTION: Math.mulDiv(uint256,uint256,uint256) (NodeID: 21)
  │         💬 Args: [x, y, denominator]
  │         👁️  Def: internal
  │       ├─ [5] ⚙️ FUNCTION: Math.mul512(uint256,uint256) (NodeID: 22)
  │       │   💬 Args: [x, y]
  │       │   👁️  Def: internal
  │       └─ [5] ⚙️ FUNCTION: Panic.panic(uint256) (NodeID: 23)
  │           💬 Args: [ternary(denominator == 0, Panic.DIVISION_BY_ZERO, Panic.UNDER_OVERFLOW)]
  │           👁️  Def: internal
  │         └─ [6] ⚙️ FUNCTION: Math.ternary(bool,uint256,uint256) (NodeID: 24)
  │             💬 Args: [denominator == 0, Panic.DIVISION_BY_ZERO, Panic.UNDER_OVERFLOW]
  │             👁️  Def: internal
  │           └─ [7] ⚙️ FUNCTION: SafeCast.toUint(bool) (NodeID: 25)
  │               💬 Args: [condition]
  │               👁️  Def: internal
  └─ [1] ⚙️ FUNCTION: Math.min(uint256,uint256) (NodeID: 30)
      💬 Args: [maxMintAmount, super.maxMint(receiver)]
      👁️  Def: internal
    ├─ [2] ⚙️ FUNCTION: BaseVault.maxMint(address) (NodeID: 33)
    │   💬 Args: [receiver]
    │   👁️  Def: public
    │ └─ [3] ⚙️ FUNCTION: ERC4626Upgradeable.convertToShares(uint256) (NodeID: 34)
    │     💬 Args: [maxDeposit(receiver)]
    │     👁️  Def: public
    │   ├─ [4] ⚙️ FUNCTION: SizeMetaVault.maxDeposit(address) (NodeID: 48)
    │   │   💬 Args: [receiver]
    │   │   👁️  Def: public
    │   │ └─ [5] ⚙️ FUNCTION: Math.min(uint256,uint256) (NodeID: 49)
    │   │     💬 Args: [_maxDeposit(), super.maxDeposit(receiver)]
    │   │     👁️  Def: internal
    │   │   ├─ [6] ⚙️ FUNCTION: SizeMetaVault._maxDeposit() (NodeID: 52)
    │   │   │   💬 Args: [no args]
    │   │   │   👁️  Def: private
    │   │   │ └─ [7] ⚙️ FUNCTION: Math.saturatingAdd(uint256,uint256) (NodeID: 53)
    │   │   │     💬 Args: [max, strategyMaxDeposit]
    │   │   │     👁️  Def: internal
    │   │   │   ├─ [8] ⚙️ FUNCTION: Math.tryAdd(uint256,uint256) (NodeID: 54)
    │   │   │   │   💬 Args: [a, b]
    │   │   │   │   👁️  Def: internal
    │   │   │   │ └─ [9] ⚙️ FUNCTION: SafeCast.toUint(bool) (NodeID: 55)
    │   │   │   │     💬 Args: [success]
    │   │   │   │     👁️  Def: internal
    │   │   │   └─ [8] ⚙️ FUNCTION: Math.ternary(bool,uint256,uint256) (NodeID: 56)
    │   │   │       💬 Args: [success, result, type(uint256).max]
    │   │   │       👁️  Def: internal
    │   │   │     └─ [9] ⚙️ FUNCTION: SafeCast.toUint(bool) (NodeID: 57)
    │   │   │         💬 Args: [condition]
    │   │   │         👁️  Def: internal
    │   │   ├─ [6] ⚙️ FUNCTION: BaseVault.maxDeposit(address) (NodeID: 58)
    │   │   │   💬 Args: [receiver]
    │   │   │   👁️  Def: public
    │   │   │ └─ [7] ⚙️ FUNCTION: Math.saturatingSub(uint256,uint256) (NodeID: 59)
    │   │   │     💬 Args: [totalAssetsCap, totalAssets()]
    │   │   │     👁️  Def: internal
    │   │   │   ├─ [8] ⚙️ FUNCTION: SizeMetaVault.totalAssets() (NodeID: 62)
    │   │   │   │   💬 Args: [no args]
    │   │   │   │   👁️  Def: public
    │   │   │   └─ [8] ⚙️ FUNCTION: Math.trySub(uint256,uint256) (NodeID: 60)
    │   │   │       💬 Args: [a, b]
    │   │   │       👁️  Def: internal
    │   │   │     └─ [9] ⚙️ FUNCTION: SafeCast.toUint(bool) (NodeID: 61)
    │   │   │         💬 Args: [success]
    │   │   │         👁️  Def: internal
    │   │   └─ [6] ⚙️ FUNCTION: Math.ternary(bool,uint256,uint256) (NodeID: 50)
    │   │       💬 Args: [a < b, a, b]
    │   │       👁️  Def: internal
    │   │     └─ [7] ⚙️ FUNCTION: SafeCast.toUint(bool) (NodeID: 51)
    │   │         💬 Args: [condition]
    │   │         👁️  Def: internal
    │   └─ [4] ⚙️ FUNCTION: ERC4626Upgradeable._convertToShares(uint256,enum Math.Rounding) (NodeID: 35)
    │       💬 Args: [assets, Math.Rounding.Floor]
    │       👁️  Def: internal
    │     └─ [5] ⚙️ FUNCTION: Math.mulDiv(uint256,uint256,uint256,enum Math.Rounding) (NodeID: 36)
    │         💬 Args: [assets, totalSupply() + (10 ** _decimalsOffset()), totalAssets() + 1, rounding]
    │         👁️  Def: internal
    │       ├─ [6] ⚙️ FUNCTION: ERC4626Upgradeable._decimalsOffset() (NodeID: 44)
    │       │   💬 Args: [no args]
    │       │   👁️  Def: internal
    │       ├─ [6] ⚙️ FUNCTION: ERC20Upgradeable.totalSupply() (NodeID: 45)
    │       │   💬 Args: [no args]
    │       │   👁️  Def: public
    │       │ └─ [7] ⚙️ FUNCTION: ERC20Upgradeable._getERC20Storage() (NodeID: 46)
    │       │     💬 Args: [no args]
    │       │     👁️  Def: private
    │       ├─ [6] ⚙️ FUNCTION: SizeMetaVault.totalAssets() (NodeID: 47)
    │       │   💬 Args: [no args]
    │       │   👁️  Def: public
    │       ├─ [6] ⚙️ FUNCTION: SafeCast.toUint(bool) (NodeID: 37)
    │       │   💬 Args: [unsignedRoundsUp(rounding) && (mulmod(x, y, denominator) > 0)]
    │       │   👁️  Def: internal
    │       │ └─ [7] ⚙️ FUNCTION: Math.unsignedRoundsUp(enum Math.Rounding) (NodeID: 38)
    │       │     💬 Args: [rounding]
    │       │     👁️  Def: internal
    │       └─ [6] ⚙️ FUNCTION: Math.mulDiv(uint256,uint256,uint256) (NodeID: 39)
    │           💬 Args: [x, y, denominator]
    │           👁️  Def: internal
    │         ├─ [7] ⚙️ FUNCTION: Math.mul512(uint256,uint256) (NodeID: 40)
    │         │   💬 Args: [x, y]
    │         │   👁️  Def: internal
    │         └─ [7] ⚙️ FUNCTION: Panic.panic(uint256) (NodeID: 41)
    │             💬 Args: [ternary(denominator == 0, Panic.DIVISION_BY_ZERO, Panic.UNDER_OVERFLOW)]
    │             👁️  Def: internal
    │           └─ [8] ⚙️ FUNCTION: Math.ternary(bool,uint256,uint256) (NodeID: 42)
    │               💬 Args: [denominator == 0, Panic.DIVISION_BY_ZERO, Panic.UNDER_OVERFLOW]
    │               👁️  Def: internal
    │             └─ [9] ⚙️ FUNCTION: SafeCast.toUint(bool) (NodeID: 43)
    │                 💬 Args: [condition]
    │                 👁️  Def: internal
    └─ [2] ⚙️ FUNCTION: Math.ternary(bool,uint256,uint256) (NodeID: 31)
        💬 Args: [a < b, a, b]
        👁️  Def: internal
      └─ [3] ⚙️ FUNCTION: SafeCast.toUint(bool) (NodeID: 32)
          💬 Args: [condition]
          👁️  Def: internal
```

## Documentation

### Function Documentation

@notice Returns the maximum number of shares that can be minted
 @dev Converts the max deposit amount to shares

### Interface Documentation

 @dev Returns the maximum amount of the Vault shares that can be minted for the receiver, through a mint call.
 - MUST return a limited value if receiver is subject to some mint limit.
 - MUST return 2 ** 256 - 1 if there is no limit on the maximum amount of shares that may be minted.
 - MUST NOT revert.
