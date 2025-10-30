# Function: maxMint(address)

**Contract**: [src/strategies/ERC4626StrategyVault.sol/contract_ERC4626StrategyVault.md]

## Metadata

- **Contract**: ERC4626StrategyVault
- **Signature**: `maxMint(address)`
- **Visibility**: public
- **Source Range**: 2795:459:148

## Implementation

```solidity
/// @notice Returns the maximum number of shares that can be minted
function maxMint(address receiver) override(BaseVault) public view returns (uint256) {
    uint256 maxDepositReceiver = maxDeposit(receiver);
    uint256 maxDepositInShares = (maxDepositReceiver == type(uint256).max) ? type(uint256).max : _convertToShares(maxDepositReceiver, Math.Rounding.Floor);
    return Math.min(maxDepositInShares, super.maxMint(receiver));
}
```

## Related Implementations

### maxDeposit(address)

- **Kind**: internal
- **Source**: 2534:183:148
- **Link**: `src/strategies/ERC4626StrategyVault.sol:ERC4626StrategyVault:maxDeposit(address)`

```solidity
/// @notice Returns the maximum amount that can be deposited
function maxDeposit(address receiver) override(BaseVault) public view returns (uint256) {
    return Math.min(vault().maxDeposit(address(this)), super.maxDeposit(receiver));
}
```

### min(uint256,uint256)

- **Kind**: internal
- **Source**: 5617:111:109
- **Link**: `lib/openzeppelin-contracts/contracts/utils/math/Math.sol:Math:min(uint256,uint256)`

```solidity
///  @dev Returns the smallest of two numbers.
function min(uint256 a, uint256 b) internal pure returns (uint256) {
    return ternary(a < b, a, b);
}
```

### vault()

- **Kind**: internal
- **Source**: 5486:112:148
- **Link**: `src/strategies/ERC4626StrategyVault.sol:ERC4626StrategyVault:vault()`

```solidity
/// @notice Returns the external vault
function vault() public view returns (IERC4626) {
    return _getERC4626StrategyVaultStorage()._vault;
}
```

### _getERC4626StrategyVaultStorage()

- **Kind**: internal
- **Source**: 1444:198:148
- **Link**: `src/strategies/ERC4626StrategyVault.sol:ERC4626StrategyVault:_getERC4626StrategyVaultStorage()`

```solidity
function _getERC4626StrategyVaultStorage() private pure returns (ERC4626StrategyVaultStorage storage $) {
    assembly {
        $.slot := ERC4626StrategyVaultStorageLocation
    }
}
```

### maxDeposit(address)

- **Kind**: internal
- **Source**: 12364:318:149
- **Link**: `src/utils/BaseVault.sol:BaseVault:maxDeposit(address)`

```solidity
/// @inheritdoc ERC4626Upgradeable
function maxDeposit(address receiver) virtual override(ERC4626Upgradeable, IERC4626) public view returns (uint256) {
    return _pausedOrAuthPaused() ? 0 : ((_totalAssetsCap() == type(uint256).max) ? super.maxDeposit(receiver) : _maxDeposit());
}
```

### _pausedOrAuthPaused()

- **Kind**: internal
- **Source**: 8334:110:149
- **Link**: `src/utils/BaseVault.sol:BaseVault:_pausedOrAuthPaused()`

```solidity
/// @notice Returns true if the vault is paused
///  @dev Checks both local pause state and global pause state from Auth
function _pausedOrAuthPaused() private view returns (bool) {
    return paused() || auth().paused();
}
```

### auth()

- **Kind**: internal
- **Source**: 14480:104:149
- **Link**: `src/utils/BaseVault.sol:BaseVault:auth()`

```solidity
/// @inheritdoc IVault
function auth() override public view returns (Auth) {
    return _getBaseVaultStorage()._auth;
}
```

### _getBaseVaultStorage()

- **Kind**: internal
- **Source**: 2552:165:149
- **Link**: `src/utils/BaseVault.sol:BaseVault:_getBaseVaultStorage()`

```solidity
function _getBaseVaultStorage() private pure returns (BaseVaultStorage storage $) {
    assembly {
        $.slot := BaseVaultStorageLocation
    }
}
```

### paused()

- **Kind**: internal
- **Source**: 2496:145:64
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/utils/PausableUpgradeable.sol:PausableUpgradeable:paused()`

```solidity
///  @dev Returns true if the contract is paused, and false otherwise.
function paused() virtual public view returns (bool) {
    PausableStorage storage $ = _getPausableStorage();
    return $._paused;
}
```

### _getPausableStorage()

- **Kind**: internal
- **Source**: 1147:162:64
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/utils/PausableUpgradeable.sol:PausableUpgradeable:_getPausableStorage()`

```solidity
function _getPausableStorage() private pure returns (PausableStorage storage $) {
    assembly {
        $.slot := PausableStorageLocation
    }
}
```

### _totalAssetsCap()

- **Kind**: internal
- **Source**: 14811:120:149
- **Link**: `src/utils/BaseVault.sol:BaseVault:_totalAssetsCap()`

```solidity
/// @notice Internal function to return the total assets cap
function _totalAssetsCap() private view returns (uint256) {
    return _getBaseVaultStorage()._totalAssetsCap;
}
```

### maxDeposit(address)

- **Kind**: internal
- **Source**: 7663:108:60
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/token/ERC20/extensions/ERC4626Upgradeable.sol:ERC4626Upgradeable:maxDeposit(address)`

```solidity
/// @dev See {IERC4626-maxDeposit}. 
function maxDeposit(address) virtual public view returns (uint256) {
    return type(uint256).max;
}
```

### _maxDeposit()

- **Kind**: internal
- **Source**: 14295:130:149
- **Link**: `src/utils/BaseVault.sol:BaseVault:_maxDeposit()`

```solidity
/// @notice Internal function to calculate the maximum amount that can be deposited
///  @dev The maximum amount that can be deposited is the total assets cap minus the total assets
function _maxDeposit() private view returns (uint256) {
    return Math.saturatingSub(_totalAssetsCap(), totalAssets());
}
```

### saturatingSub(uint256,uint256)

- **Kind**: internal
- **Source**: 4217:150:109
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
- **Source**: 4255:181:148
- **Link**: `src/strategies/ERC4626StrategyVault.sol:ERC4626StrategyVault:totalAssets()`

```solidity
/// @notice Returns the total assets managed by this strategy
///  @dev Converts the external vault shares held by this contract to asset value
///  @return The total assets under management
function totalAssets() virtual override(ERC4626Upgradeable, IERC4626) public view returns (uint256) {
    return vault().convertToAssets(vault().balanceOf(address(this)));
}
```

### trySub(uint256,uint256)

- **Kind**: internal
- **Source**: 2052:240:109
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

### toUint(bool)

- **Kind**: internal
- **Source**: 34795:145:110
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
- **Source**: 5071:294:109
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

### _convertToShares(uint256,enum Math.Rounding)

- **Kind**: internal
- **Source**: 10972:213:60
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/token/ERC20/extensions/ERC4626Upgradeable.sol:ERC4626Upgradeable:_convertToShares(uint256,enum Math.Rounding)`

```solidity
///  @dev Internal conversion function (from assets to shares) with support for rounding direction.
function _convertToShares(uint256 assets, Math.Rounding rounding) virtual internal view returns (uint256) {
    return assets.mulDiv(totalSupply() + (10 ** _decimalsOffset()), totalAssets() + 1, rounding);
}
```

### mulDiv(uint256,uint256,uint256,enum Math.Rounding)

- **Kind**: internal
- **Source**: 11054:238:109
- **Link**: `lib/openzeppelin-contracts/contracts/utils/math/Math.sol:Math:mulDiv(uint256,uint256,uint256,enum Math.Rounding)`

```solidity
///  @dev Calculates x * y / denominator with full precision, following the selected rounding direction.
function mulDiv(uint256 x, uint256 y, uint256 denominator, Rounding rounding) internal pure returns (uint256) {
    return mulDiv(x, y, denominator) + SafeCast.toUint(unsignedRoundsUp(rounding) && (mulmod(x, y, denominator) > 0));
}
```

### _decimalsOffset()

- **Kind**: internal
- **Source**: 13425:90:60
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/token/ERC20/extensions/ERC4626Upgradeable.sol:ERC4626Upgradeable:_decimalsOffset()`

```solidity
function _decimalsOffset() virtual internal view returns (uint8) {
    return 0;
}
```

### totalSupply()

- **Kind**: internal
- **Source**: 3877:152:58
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
- **Source**: 1947:153:58
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
- **Source**: 32020:122:109
- **Link**: `lib/openzeppelin-contracts/contracts/utils/math/Math.sol:Math:unsignedRoundsUp(enum Math.Rounding)`

```solidity
///  @dev Returns whether a provided rounding mode is considered rounding up for unsigned integers.
function unsignedRoundsUp(Rounding rounding) internal pure returns (bool) {
    return (uint8(rounding) % 2) == 1;
}
```

### mulDiv(uint256,uint256,uint256)

- **Kind**: internal
- **Source**: 7242:3683:109
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
- **Source**: 1027:550:109
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
- **Source**: 1776:194:101
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
- **Source**: 12727:339:149
- **Link**: `src/utils/BaseVault.sol:BaseVault:maxMint(address)`

```solidity
/// @inheritdoc ERC4626Upgradeable
function maxMint(address receiver) virtual override(ERC4626Upgradeable, IERC4626) public view returns (uint256) {
    return _pausedOrAuthPaused() ? 0 : ((_totalAssetsCap() == type(uint256).max) ? super.maxMint(receiver) : _convertToShares(_maxDeposit(), Math.Rounding.Floor));
}
```

### maxMint(address)

- **Kind**: internal
- **Source**: 7817:105:60
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/token/ERC20/extensions/ERC4626Upgradeable.sol:ERC4626Upgradeable:maxMint(address)`

```solidity
/// @dev See {IERC4626-maxMint}. 
function maxMint(address) virtual public view returns (uint256) {
    return type(uint256).max;
}
```

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: ERC4626StrategyVault.maxMint(address) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
  ├─ [1] ⚙️ FUNCTION: ERC4626StrategyVault.maxDeposit(address) (NodeID: 1)
  │   💬 Args: [receiver]
  │   👁️  Def: public
  │ └─ [2] ⚙️ FUNCTION: Math.min(uint256,uint256) (NodeID: 2)
  │     💬 Args: [vault().maxDeposit(address(this)), super.maxDeposit(receiver)]
  │     👁️  Def: internal
  │   ├─ [3] ⚙️ FUNCTION: ERC4626StrategyVault.vault() (NodeID: 5)
  │   │   💬 Args: [no args]
  │   │   👁️  Def: public
  │   │ └─ [4] ⚙️ FUNCTION: ERC4626StrategyVault._getERC4626StrategyVaultStorage() (NodeID: 6)
  │   │     💬 Args: [no args]
  │   │     👁️  Def: private
  │   ├─ [3] ⚙️ FUNCTION: BaseVault.maxDeposit(address) (NodeID: 7)
  │   │   💬 Args: [receiver]
  │   │   👁️  Def: public
  │   │ ├─ [4] ⚙️ FUNCTION: BaseVault._pausedOrAuthPaused() (NodeID: 8)
  │   │ │   💬 Args: [no args]
  │   │ │   👁️  Def: private
  │   │ │ ├─ [5] ⚙️ FUNCTION: BaseVault.auth() (NodeID: 9)
  │   │ │ │   💬 Args: [no args]
  │   │ │ │   👁️  Def: public
  │   │ │ │ └─ [6] ⚙️ FUNCTION: BaseVault._getBaseVaultStorage() (NodeID: 10)
  │   │ │ │     💬 Args: [no args]
  │   │ │ │     👁️  Def: private
  │   │ │ └─ [5] ⚙️ FUNCTION: PausableUpgradeable.paused() (NodeID: 11)
  │   │ │     💬 Args: [no args]
  │   │ │     👁️  Def: public
  │   │ │   └─ [6] ⚙️ FUNCTION: PausableUpgradeable._getPausableStorage() (NodeID: 12)
  │   │ │       💬 Args: [no args]
  │   │ │       👁️  Def: private
  │   │ ├─ [4] ⚙️ FUNCTION: BaseVault._totalAssetsCap() (NodeID: 13)
  │   │ │   💬 Args: [no args]
  │   │ │   👁️  Def: private
  │   │ │ └─ [5] ⚙️ FUNCTION: BaseVault._getBaseVaultStorage() (NodeID: 14)
  │   │ │     💬 Args: [no args]
  │   │ │     👁️  Def: private
  │   │ ├─ [4] ⚙️ FUNCTION: ERC4626Upgradeable.maxDeposit(address) (NodeID: 15)
  │   │ │   💬 Args: [receiver]
  │   │ │   👁️  Def: public
  │   │ └─ [4] ⚙️ FUNCTION: BaseVault._maxDeposit() (NodeID: 16)
  │   │     💬 Args: [no args]
  │   │     👁️  Def: private
  │   │   └─ [5] ⚙️ FUNCTION: Math.saturatingSub(uint256,uint256) (NodeID: 17)
  │   │       💬 Args: [_totalAssetsCap(), totalAssets()]
  │   │       👁️  Def: internal
  │   │     ├─ [6] ⚙️ FUNCTION: BaseVault._totalAssetsCap() (NodeID: 20)
  │   │     │   💬 Args: [no args]
  │   │     │   👁️  Def: private
  │   │     │ └─ [7] ⚙️ FUNCTION: BaseVault._getBaseVaultStorage() (NodeID: 21)
  │   │     │     💬 Args: [no args]
  │   │     │     👁️  Def: private
  │   │     ├─ [6] ⚙️ FUNCTION: ERC4626StrategyVault.totalAssets() (NodeID: 22)
  │   │     │   💬 Args: [no args]
  │   │     │   👁️  Def: public
  │   │     │ ├─ [7] ⚙️ FUNCTION: ERC4626StrategyVault.vault() (NodeID: 23)
  │   │     │ │   💬 Args: [no args]
  │   │     │ │   👁️  Def: public
  │   │     │ │ └─ [8] ⚙️ FUNCTION: ERC4626StrategyVault._getERC4626StrategyVaultStorage() (NodeID: 24)
  │   │     │ │     💬 Args: [no args]
  │   │     │ │     👁️  Def: private
  │   │     │ └─ [7] ⚙️ FUNCTION: ERC4626StrategyVault.vault() (NodeID: 25)
  │   │     │     💬 Args: [no args]
  │   │     │     👁️  Def: public
  │   │     │   └─ [8] ⚙️ FUNCTION: ERC4626StrategyVault._getERC4626StrategyVaultStorage() (NodeID: 26)
  │   │     │       💬 Args: [no args]
  │   │     │       👁️  Def: private
  │   │     └─ [6] ⚙️ FUNCTION: Math.trySub(uint256,uint256) (NodeID: 18)
  │   │         💬 Args: [a, b]
  │   │         👁️  Def: internal
  │   │       └─ [7] ⚙️ FUNCTION: SafeCast.toUint(bool) (NodeID: 19)
  │   │           💬 Args: [success]
  │   │           👁️  Def: internal
  │   └─ [3] ⚙️ FUNCTION: Math.ternary(bool,uint256,uint256) (NodeID: 3)
  │       💬 Args: [a < b, a, b]
  │       👁️  Def: internal
  │     └─ [4] ⚙️ FUNCTION: SafeCast.toUint(bool) (NodeID: 4)
  │         💬 Args: [condition]
  │         👁️  Def: internal
  ├─ [1] ⚙️ FUNCTION: ERC4626Upgradeable._convertToShares(uint256,enum Math.Rounding) (NodeID: 27)
  │   💬 Args: [maxDepositReceiver, Math.Rounding.Floor]
  │   👁️  Def: internal
  │ └─ [2] ⚙️ FUNCTION: Math.mulDiv(uint256,uint256,uint256,enum Math.Rounding) (NodeID: 28)
  │     💬 Args: [assets, totalSupply() + (10 ** _decimalsOffset()), totalAssets() + 1, rounding]
  │     👁️  Def: internal
  │   ├─ [3] ⚙️ FUNCTION: ERC4626Upgradeable._decimalsOffset() (NodeID: 36)
  │   │   💬 Args: [no args]
  │   │   👁️  Def: internal
  │   ├─ [3] ⚙️ FUNCTION: ERC20Upgradeable.totalSupply() (NodeID: 37)
  │   │   💬 Args: [no args]
  │   │   👁️  Def: public
  │   │ └─ [4] ⚙️ FUNCTION: ERC20Upgradeable._getERC20Storage() (NodeID: 38)
  │   │     💬 Args: [no args]
  │   │     👁️  Def: private
  │   ├─ [3] ⚙️ FUNCTION: ERC4626StrategyVault.totalAssets() (NodeID: 39)
  │   │   💬 Args: [no args]
  │   │   👁️  Def: public
  │   │ ├─ [4] ⚙️ FUNCTION: ERC4626StrategyVault.vault() (NodeID: 40)
  │   │ │   💬 Args: [no args]
  │   │ │   👁️  Def: public
  │   │ │ └─ [5] ⚙️ FUNCTION: ERC4626StrategyVault._getERC4626StrategyVaultStorage() (NodeID: 41)
  │   │ │     💬 Args: [no args]
  │   │ │     👁️  Def: private
  │   │ └─ [4] ⚙️ FUNCTION: ERC4626StrategyVault.vault() (NodeID: 42)
  │   │     💬 Args: [no args]
  │   │     👁️  Def: public
  │   │   └─ [5] ⚙️ FUNCTION: ERC4626StrategyVault._getERC4626StrategyVaultStorage() (NodeID: 43)
  │   │       💬 Args: [no args]
  │   │       👁️  Def: private
  │   ├─ [3] ⚙️ FUNCTION: SafeCast.toUint(bool) (NodeID: 29)
  │   │   💬 Args: [unsignedRoundsUp(rounding) && (mulmod(x, y, denominator) > 0)]
  │   │   👁️  Def: internal
  │   │ └─ [4] ⚙️ FUNCTION: Math.unsignedRoundsUp(enum Math.Rounding) (NodeID: 30)
  │   │     💬 Args: [rounding]
  │   │     👁️  Def: internal
  │   └─ [3] ⚙️ FUNCTION: Math.mulDiv(uint256,uint256,uint256) (NodeID: 31)
  │       💬 Args: [x, y, denominator]
  │       👁️  Def: internal
  │     ├─ [4] ⚙️ FUNCTION: Math.mul512(uint256,uint256) (NodeID: 32)
  │     │   💬 Args: [x, y]
  │     │   👁️  Def: internal
  │     └─ [4] ⚙️ FUNCTION: Panic.panic(uint256) (NodeID: 33)
  │         💬 Args: [ternary(denominator == 0, Panic.DIVISION_BY_ZERO, Panic.UNDER_OVERFLOW)]
  │         👁️  Def: internal
  │       └─ [5] ⚙️ FUNCTION: Math.ternary(bool,uint256,uint256) (NodeID: 34)
  │           💬 Args: [denominator == 0, Panic.DIVISION_BY_ZERO, Panic.UNDER_OVERFLOW]
  │           👁️  Def: internal
  │         └─ [6] ⚙️ FUNCTION: SafeCast.toUint(bool) (NodeID: 35)
  │             💬 Args: [condition]
  │             👁️  Def: internal
  └─ [1] ⚙️ FUNCTION: Math.min(uint256,uint256) (NodeID: 44)
      💬 Args: [maxDepositInShares, super.maxMint(receiver)]
      👁️  Def: internal
    ├─ [2] ⚙️ FUNCTION: BaseVault.maxMint(address) (NodeID: 47)
    │   💬 Args: [receiver]
    │   👁️  Def: public
    │ ├─ [3] ⚙️ FUNCTION: BaseVault._pausedOrAuthPaused() (NodeID: 48)
    │ │   💬 Args: [no args]
    │ │   👁️  Def: private
    │ │ ├─ [4] ⚙️ FUNCTION: BaseVault.auth() (NodeID: 49)
    │ │ │   💬 Args: [no args]
    │ │ │   👁️  Def: public
    │ │ │ └─ [5] ⚙️ FUNCTION: BaseVault._getBaseVaultStorage() (NodeID: 50)
    │ │ │     💬 Args: [no args]
    │ │ │     👁️  Def: private
    │ │ └─ [4] ⚙️ FUNCTION: PausableUpgradeable.paused() (NodeID: 51)
    │ │     💬 Args: [no args]
    │ │     👁️  Def: public
    │ │   └─ [5] ⚙️ FUNCTION: PausableUpgradeable._getPausableStorage() (NodeID: 52)
    │ │       💬 Args: [no args]
    │ │       👁️  Def: private
    │ ├─ [3] ⚙️ FUNCTION: BaseVault._totalAssetsCap() (NodeID: 53)
    │ │   💬 Args: [no args]
    │ │   👁️  Def: private
    │ │ └─ [4] ⚙️ FUNCTION: BaseVault._getBaseVaultStorage() (NodeID: 54)
    │ │     💬 Args: [no args]
    │ │     👁️  Def: private
    │ ├─ [3] ⚙️ FUNCTION: ERC4626Upgradeable.maxMint(address) (NodeID: 55)
    │ │   💬 Args: [receiver]
    │ │   👁️  Def: public
    │ └─ [3] ⚙️ FUNCTION: ERC4626Upgradeable._convertToShares(uint256,enum Math.Rounding) (NodeID: 56)
    │     💬 Args: [_maxDeposit(), Math.Rounding.Floor]
    │     👁️  Def: internal
    │   ├─ [4] ⚙️ FUNCTION: BaseVault._maxDeposit() (NodeID: 73)
    │   │   💬 Args: [no args]
    │   │   👁️  Def: private
    │   │ └─ [5] ⚙️ FUNCTION: Math.saturatingSub(uint256,uint256) (NodeID: 74)
    │   │     💬 Args: [_totalAssetsCap(), totalAssets()]
    │   │     👁️  Def: internal
    │   │   ├─ [6] ⚙️ FUNCTION: BaseVault._totalAssetsCap() (NodeID: 77)
    │   │   │   💬 Args: [no args]
    │   │   │   👁️  Def: private
    │   │   │ └─ [7] ⚙️ FUNCTION: BaseVault._getBaseVaultStorage() (NodeID: 78)
    │   │   │     💬 Args: [no args]
    │   │   │     👁️  Def: private
    │   │   ├─ [6] ⚙️ FUNCTION: ERC4626StrategyVault.totalAssets() (NodeID: 79)
    │   │   │   💬 Args: [no args]
    │   │   │   👁️  Def: public
    │   │   │ ├─ [7] ⚙️ FUNCTION: ERC4626StrategyVault.vault() (NodeID: 80)
    │   │   │ │   💬 Args: [no args]
    │   │   │ │   👁️  Def: public
    │   │   │ │ └─ [8] ⚙️ FUNCTION: ERC4626StrategyVault._getERC4626StrategyVaultStorage() (NodeID: 81)
    │   │   │ │     💬 Args: [no args]
    │   │   │ │     👁️  Def: private
    │   │   │ └─ [7] ⚙️ FUNCTION: ERC4626StrategyVault.vault() (NodeID: 82)
    │   │   │     💬 Args: [no args]
    │   │   │     👁️  Def: public
    │   │   │   └─ [8] ⚙️ FUNCTION: ERC4626StrategyVault._getERC4626StrategyVaultStorage() (NodeID: 83)
    │   │   │       💬 Args: [no args]
    │   │   │       👁️  Def: private
    │   │   └─ [6] ⚙️ FUNCTION: Math.trySub(uint256,uint256) (NodeID: 75)
    │   │       💬 Args: [a, b]
    │   │       👁️  Def: internal
    │   │     └─ [7] ⚙️ FUNCTION: SafeCast.toUint(bool) (NodeID: 76)
    │   │         💬 Args: [success]
    │   │         👁️  Def: internal
    │   └─ [4] ⚙️ FUNCTION: Math.mulDiv(uint256,uint256,uint256,enum Math.Rounding) (NodeID: 57)
    │       💬 Args: [assets, totalSupply() + (10 ** _decimalsOffset()), totalAssets() + 1, rounding]
    │       👁️  Def: internal
    │     ├─ [5] ⚙️ FUNCTION: ERC4626Upgradeable._decimalsOffset() (NodeID: 65)
    │     │   💬 Args: [no args]
    │     │   👁️  Def: internal
    │     ├─ [5] ⚙️ FUNCTION: ERC20Upgradeable.totalSupply() (NodeID: 66)
    │     │   💬 Args: [no args]
    │     │   👁️  Def: public
    │     │ └─ [6] ⚙️ FUNCTION: ERC20Upgradeable._getERC20Storage() (NodeID: 67)
    │     │     💬 Args: [no args]
    │     │     👁️  Def: private
    │     ├─ [5] ⚙️ FUNCTION: ERC4626StrategyVault.totalAssets() (NodeID: 68)
    │     │   💬 Args: [no args]
    │     │   👁️  Def: public
    │     │ ├─ [6] ⚙️ FUNCTION: ERC4626StrategyVault.vault() (NodeID: 69)
    │     │ │   💬 Args: [no args]
    │     │ │   👁️  Def: public
    │     │ │ └─ [7] ⚙️ FUNCTION: ERC4626StrategyVault._getERC4626StrategyVaultStorage() (NodeID: 70)
    │     │ │     💬 Args: [no args]
    │     │ │     👁️  Def: private
    │     │ └─ [6] ⚙️ FUNCTION: ERC4626StrategyVault.vault() (NodeID: 71)
    │     │     💬 Args: [no args]
    │     │     👁️  Def: public
    │     │   └─ [7] ⚙️ FUNCTION: ERC4626StrategyVault._getERC4626StrategyVaultStorage() (NodeID: 72)
    │     │       💬 Args: [no args]
    │     │       👁️  Def: private
    │     ├─ [5] ⚙️ FUNCTION: SafeCast.toUint(bool) (NodeID: 58)
    │     │   💬 Args: [unsignedRoundsUp(rounding) && (mulmod(x, y, denominator) > 0)]
    │     │   👁️  Def: internal
    │     │ └─ [6] ⚙️ FUNCTION: Math.unsignedRoundsUp(enum Math.Rounding) (NodeID: 59)
    │     │     💬 Args: [rounding]
    │     │     👁️  Def: internal
    │     └─ [5] ⚙️ FUNCTION: Math.mulDiv(uint256,uint256,uint256) (NodeID: 60)
    │         💬 Args: [x, y, denominator]
    │         👁️  Def: internal
    │       ├─ [6] ⚙️ FUNCTION: Math.mul512(uint256,uint256) (NodeID: 61)
    │       │   💬 Args: [x, y]
    │       │   👁️  Def: internal
    │       └─ [6] ⚙️ FUNCTION: Panic.panic(uint256) (NodeID: 62)
    │           💬 Args: [ternary(denominator == 0, Panic.DIVISION_BY_ZERO, Panic.UNDER_OVERFLOW)]
    │           👁️  Def: internal
    │         └─ [7] ⚙️ FUNCTION: Math.ternary(bool,uint256,uint256) (NodeID: 63)
    │             💬 Args: [denominator == 0, Panic.DIVISION_BY_ZERO, Panic.UNDER_OVERFLOW]
    │             👁️  Def: internal
    │           └─ [8] ⚙️ FUNCTION: SafeCast.toUint(bool) (NodeID: 64)
    │               💬 Args: [condition]
    │               👁️  Def: internal
    └─ [2] ⚙️ FUNCTION: Math.ternary(bool,uint256,uint256) (NodeID: 45)
        💬 Args: [a < b, a, b]
        👁️  Def: internal
      └─ [3] ⚙️ FUNCTION: SafeCast.toUint(bool) (NodeID: 46)
          💬 Args: [condition]
          👁️  Def: internal
```

## Documentation

### Function Documentation

@notice Returns the maximum number of shares that can be minted

### Interface Documentation

 @dev Returns the maximum amount of the Vault shares that can be minted for the receiver, through a mint call.
 - MUST return a limited value if receiver is subject to some mint limit.
 - MUST return 2 ** 256 - 1 if there is no limit on the maximum amount of shares that may be minted.
 - MUST NOT revert.
