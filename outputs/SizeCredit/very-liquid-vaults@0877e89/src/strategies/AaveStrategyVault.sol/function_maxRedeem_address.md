# Function: maxRedeem(address)

**Contract**: [src/strategies/AaveStrategyVault.sol/contract_AaveStrategyVault.md]

## Metadata

- **Contract**: AaveStrategyVault
- **Signature**: `maxRedeem(address)`
- **Visibility**: public
- **Source Range**: 6285:602:146

## Implementation

```solidity
/// @inheritdoc ERC4626Upgradeable
///  @dev Limited by both owner's balance and Aave pool liquidity
function maxRedeem(address owner) override(BaseVault) public view returns (uint256) {
    DataTypes.ReserveConfigurationMap memory config = pool().getReserveData(asset()).configuration;
    if (!(config.getActive() && (!config.getPaused()))) return 0;
    uint256 cash = IERC20(asset()).balanceOf(address(aToken()));
    uint256 cashInShares = _convertToShares(cash, Math.Rounding.Floor);
    uint256 shareBalance = balanceOf(owner);
    return Math.min((cashInShares < shareBalance) ? cashInShares : shareBalance, super.maxRedeem(owner));
}
```

## Related Implementations

### pool()

- **Kind**: internal
- **Source**: 8500:104:146
- **Link**: `src/strategies/AaveStrategyVault.sol:AaveStrategyVault:pool()`

```solidity
/// @notice Returns the Aave pool
///  @return The Aave pool
function pool() public view returns (IPool) {
    return _getAaveStrategyVaultStorage()._pool;
}
```

### _getAaveStrategyVaultStorage()

- **Kind**: internal
- **Source**: 2083:189:146
- **Link**: `src/strategies/AaveStrategyVault.sol:AaveStrategyVault:_getAaveStrategyVaultStorage()`

```solidity
function _getAaveStrategyVaultStorage() private pure returns (AaveStrategyVaultStorage storage $) {
    assembly {
        $.slot := AaveStrategyVaultStorageLocation
    }
}
```

### asset()

- **Kind**: internal
- **Source**: 6882:153:60
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/token/ERC20/extensions/ERC4626Upgradeable.sol:ERC4626Upgradeable:asset()`

```solidity
/// @dev See {IERC4626-asset}. 
function asset() virtual public view returns (address) {
    ERC4626Storage storage $ = _getERC4626Storage();
    return address($._asset);
}
```

### _getERC4626Storage()

- **Kind**: internal
- **Source**: 4088:159:60
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/token/ERC20/extensions/ERC4626Upgradeable.sol:ERC4626Upgradeable:_getERC4626Storage()`

```solidity
function _getERC4626Storage() private pure returns (ERC4626Storage storage $) {
    assembly {
        $.slot := ERC4626StorageLocation
    }
}
```

### getPaused(struct DataTypes.ReserveConfigurationMap)

- **Kind**: internal
- **Source**: 10223:143:27
- **Link**: `lib/aave-v3-origin/src/contracts/protocol/libraries/configuration/ReserveConfiguration.sol:ReserveConfiguration:getPaused(struct DataTypes.ReserveConfigurationMap)`

```solidity
///  @notice Gets the paused state of the reserve
///  @param self The reserve configuration
///  @return The paused state
function getPaused(DataTypes.ReserveConfigurationMap memory self) internal pure returns (bool) {
    return (self.data & PAUSED_MASK) != 0;
}
```

### getActive(struct DataTypes.ReserveConfigurationMap)

- **Kind**: internal
- **Source**: 8941:143:27
- **Link**: `lib/aave-v3-origin/src/contracts/protocol/libraries/configuration/ReserveConfiguration.sol:ReserveConfiguration:getActive(struct DataTypes.ReserveConfigurationMap)`

```solidity
///  @notice Gets the active state of the reserve
///  @param self The reserve configuration
///  @return The active state
function getActive(DataTypes.ReserveConfigurationMap memory self) internal pure returns (bool) {
    return (self.data & ACTIVE_MASK) != 0;
}
```

### aToken()

- **Kind**: internal
- **Source**: 8682:110:146
- **Link**: `src/strategies/AaveStrategyVault.sol:AaveStrategyVault:aToken()`

```solidity
/// @notice Returns the Aave aToken
///  @return The Aave aToken
function aToken() public view returns (IAToken) {
    return _getAaveStrategyVaultStorage()._aToken;
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

### totalAssets()

- **Kind**: internal
- **Source**: 7160:402:146
- **Link**: `src/strategies/AaveStrategyVault.sol:AaveStrategyVault:totalAssets()`

```solidity
/// @inheritdoc ERC4626Upgradeable
///  @dev Returns the aToken balance since aTokens represent the underlying asset with accrued interest
///  @dev Round down to avoid stealing assets in roundtrip operations https://github.com/a16z/erc4626-tests/issues/13
function totalAssets() virtual override(ERC4626Upgradeable, IERC4626) public view returns (uint256) {
    /// @notice aTokens use rebasing to accrue interest, so the total assets is just the aToken balance
    uint256 liquidityIndex = pool().getReserveNormalizedIncome(address(asset()));
    return Math.mulDiv(aToken().scaledBalanceOf(address(this)), liquidityIndex, WadRayMath.RAY);
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

### balanceOf(address)

- **Kind**: internal
- **Source**: 4087:171:58
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/token/ERC20/ERC20Upgradeable.sol:ERC20Upgradeable:balanceOf(address)`

```solidity
///  @dev See {IERC20-balanceOf}.
function balanceOf(address account) virtual public view returns (uint256) {
    ERC20Storage storage $ = _getERC20Storage();
    return $._balances[account];
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

### maxRedeem(address)

- **Kind**: internal
- **Source**: 13345:185:149
- **Link**: `src/utils/BaseVault.sol:BaseVault:maxRedeem(address)`

```solidity
/// @inheritdoc ERC4626Upgradeable
function maxRedeem(address owner) virtual override(ERC4626Upgradeable, IERC4626) public view returns (uint256) {
    return _pausedOrAuthPaused() ? 0 : super.maxRedeem(owner);
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

### maxRedeem(address)

- **Kind**: internal
- **Source**: 8173:112:60
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/token/ERC20/extensions/ERC4626Upgradeable.sol:ERC4626Upgradeable:maxRedeem(address)`

```solidity
/// @dev See {IERC4626-maxRedeem}. 
function maxRedeem(address owner) virtual public view returns (uint256) {
    return balanceOf(owner);
}
```

## External Calls

- **IPool::getReserveData(address)**
- **IERC20::balanceOf(address)**

## State Variable Reads

- **PAUSED_MASK** (`uint256`)
- **ACTIVE_MASK** (`uint256`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: AaveStrategyVault.maxRedeem(address) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
  ├─ [1] ⚙️ FUNCTION: AaveStrategyVault.pool() (NodeID: 1)
  │   💬 Args: [no args]
  │   👁️  Def: public
  │ └─ [2] ⚙️ FUNCTION: AaveStrategyVault._getAaveStrategyVaultStorage() (NodeID: 2)
  │     💬 Args: [no args]
  │     👁️  Def: private
  ├─ [1] ⚙️ FUNCTION: ERC4626Upgradeable.asset() (NodeID: 3)
  │   💬 Args: [no args]
  │   👁️  Def: public
  │ └─ [2] ⚙️ FUNCTION: ERC4626Upgradeable._getERC4626Storage() (NodeID: 4)
  │     💬 Args: [no args]
  │     👁️  Def: private
  ├─ [1] ⚙️ FUNCTION: ReserveConfiguration.getPaused(struct DataTypes.ReserveConfigurationMap) (NodeID: 5)
  │   💬 Args: [config]
  │   👁️  Def: internal
  ├─ [1] ⚙️ FUNCTION: ReserveConfiguration.getActive(struct DataTypes.ReserveConfigurationMap) (NodeID: 6)
  │   💬 Args: [config]
  │   👁️  Def: internal
  ├─ [1] ⚙️ FUNCTION: ERC4626Upgradeable.asset() (NodeID: 7)
  │   💬 Args: [no args]
  │   👁️  Def: public
  │ └─ [2] ⚙️ FUNCTION: ERC4626Upgradeable._getERC4626Storage() (NodeID: 8)
  │     💬 Args: [no args]
  │     👁️  Def: private
  ├─ [1] ⚙️ FUNCTION: AaveStrategyVault.aToken() (NodeID: 9)
  │   💬 Args: [no args]
  │   👁️  Def: public
  │ └─ [2] ⚙️ FUNCTION: AaveStrategyVault._getAaveStrategyVaultStorage() (NodeID: 10)
  │     💬 Args: [no args]
  │     👁️  Def: private
  ├─ [1] ⚙️ FUNCTION: ERC4626Upgradeable._convertToShares(uint256,enum Math.Rounding) (NodeID: 11)
  │   💬 Args: [cash, Math.Rounding.Floor]
  │   👁️  Def: internal
  │ └─ [2] ⚙️ FUNCTION: Math.mulDiv(uint256,uint256,uint256,enum Math.Rounding) (NodeID: 12)
  │     💬 Args: [assets, totalSupply() + (10 ** _decimalsOffset()), totalAssets() + 1, rounding]
  │     👁️  Def: internal
  │   ├─ [3] ⚙️ FUNCTION: ERC4626Upgradeable._decimalsOffset() (NodeID: 20)
  │   │   💬 Args: [no args]
  │   │   👁️  Def: internal
  │   ├─ [3] ⚙️ FUNCTION: ERC20Upgradeable.totalSupply() (NodeID: 21)
  │   │   💬 Args: [no args]
  │   │   👁️  Def: public
  │   │ └─ [4] ⚙️ FUNCTION: ERC20Upgradeable._getERC20Storage() (NodeID: 22)
  │   │     💬 Args: [no args]
  │   │     👁️  Def: private
  │   ├─ [3] ⚙️ FUNCTION: AaveStrategyVault.totalAssets() (NodeID: 23)
  │   │   💬 Args: [no args]
  │   │   👁️  Def: public
  │   │ ├─ [4] ⚙️ FUNCTION: AaveStrategyVault.pool() (NodeID: 24)
  │   │ │   💬 Args: [no args]
  │   │ │   👁️  Def: public
  │   │ │ └─ [5] ⚙️ FUNCTION: AaveStrategyVault._getAaveStrategyVaultStorage() (NodeID: 25)
  │   │ │     💬 Args: [no args]
  │   │ │     👁️  Def: private
  │   │ ├─ [4] ⚙️ FUNCTION: ERC4626Upgradeable.asset() (NodeID: 26)
  │   │ │   💬 Args: [no args]
  │   │ │   👁️  Def: public
  │   │ │ └─ [5] ⚙️ FUNCTION: ERC4626Upgradeable._getERC4626Storage() (NodeID: 27)
  │   │ │     💬 Args: [no args]
  │   │ │     👁️  Def: private
  │   │ └─ [4] ⚙️ FUNCTION: Math.mulDiv(uint256,uint256,uint256) (NodeID: 28)
  │   │     💬 Args: [aToken().scaledBalanceOf(address(this)), liquidityIndex, WadRayMath.RAY]
  │   │     👁️  Def: internal
  │   │   ├─ [5] ⚙️ FUNCTION: AaveStrategyVault.aToken() (NodeID: 33)
  │   │   │   💬 Args: [no args]
  │   │   │   👁️  Def: public
  │   │   │ └─ [6] ⚙️ FUNCTION: AaveStrategyVault._getAaveStrategyVaultStorage() (NodeID: 34)
  │   │   │     💬 Args: [no args]
  │   │   │     👁️  Def: private
  │   │   ├─ [5] ⚙️ FUNCTION: Math.mul512(uint256,uint256) (NodeID: 29)
  │   │   │   💬 Args: [x, y]
  │   │   │   👁️  Def: internal
  │   │   └─ [5] ⚙️ FUNCTION: Panic.panic(uint256) (NodeID: 30)
  │   │       💬 Args: [ternary(denominator == 0, Panic.DIVISION_BY_ZERO, Panic.UNDER_OVERFLOW)]
  │   │       👁️  Def: internal
  │   │     └─ [6] ⚙️ FUNCTION: Math.ternary(bool,uint256,uint256) (NodeID: 31)
  │   │         💬 Args: [denominator == 0, Panic.DIVISION_BY_ZERO, Panic.UNDER_OVERFLOW]
  │   │         👁️  Def: internal
  │   │       └─ [7] ⚙️ FUNCTION: SafeCast.toUint(bool) (NodeID: 32)
  │   │           💬 Args: [condition]
  │   │           👁️  Def: internal
  │   ├─ [3] ⚙️ FUNCTION: SafeCast.toUint(bool) (NodeID: 13)
  │   │   💬 Args: [unsignedRoundsUp(rounding) && (mulmod(x, y, denominator) > 0)]
  │   │   👁️  Def: internal
  │   │ └─ [4] ⚙️ FUNCTION: Math.unsignedRoundsUp(enum Math.Rounding) (NodeID: 14)
  │   │     💬 Args: [rounding]
  │   │     👁️  Def: internal
  │   └─ [3] ⚙️ FUNCTION: Math.mulDiv(uint256,uint256,uint256) (NodeID: 15)
  │       💬 Args: [x, y, denominator]
  │       👁️  Def: internal
  │     ├─ [4] ⚙️ FUNCTION: Math.mul512(uint256,uint256) (NodeID: 16)
  │     │   💬 Args: [x, y]
  │     │   👁️  Def: internal
  │     └─ [4] ⚙️ FUNCTION: Panic.panic(uint256) (NodeID: 17)
  │         💬 Args: [ternary(denominator == 0, Panic.DIVISION_BY_ZERO, Panic.UNDER_OVERFLOW)]
  │         👁️  Def: internal
  │       └─ [5] ⚙️ FUNCTION: Math.ternary(bool,uint256,uint256) (NodeID: 18)
  │           💬 Args: [denominator == 0, Panic.DIVISION_BY_ZERO, Panic.UNDER_OVERFLOW]
  │           👁️  Def: internal
  │         └─ [6] ⚙️ FUNCTION: SafeCast.toUint(bool) (NodeID: 19)
  │             💬 Args: [condition]
  │             👁️  Def: internal
  ├─ [1] ⚙️ FUNCTION: ERC20Upgradeable.balanceOf(address) (NodeID: 35)
  │   💬 Args: [owner]
  │   👁️  Def: public
  │ └─ [2] ⚙️ FUNCTION: ERC20Upgradeable._getERC20Storage() (NodeID: 36)
  │     💬 Args: [no args]
  │     👁️  Def: private
  └─ [1] ⚙️ FUNCTION: Math.min(uint256,uint256) (NodeID: 37)
      💬 Args: [(cashInShares < shareBalance) ? cashInShares : shareBalance, super.maxRedeem(owner)]
      👁️  Def: internal
    ├─ [2] ⚙️ FUNCTION: BaseVault.maxRedeem(address) (NodeID: 40)
    │   💬 Args: [owner]
    │   👁️  Def: public
    │ ├─ [3] ⚙️ FUNCTION: BaseVault._pausedOrAuthPaused() (NodeID: 41)
    │ │   💬 Args: [no args]
    │ │   👁️  Def: private
    │ │ ├─ [4] ⚙️ FUNCTION: BaseVault.auth() (NodeID: 42)
    │ │ │   💬 Args: [no args]
    │ │ │   👁️  Def: public
    │ │ │ └─ [5] ⚙️ FUNCTION: BaseVault._getBaseVaultStorage() (NodeID: 43)
    │ │ │     💬 Args: [no args]
    │ │ │     👁️  Def: private
    │ │ └─ [4] ⚙️ FUNCTION: PausableUpgradeable.paused() (NodeID: 44)
    │ │     💬 Args: [no args]
    │ │     👁️  Def: public
    │ │   └─ [5] ⚙️ FUNCTION: PausableUpgradeable._getPausableStorage() (NodeID: 45)
    │ │       💬 Args: [no args]
    │ │       👁️  Def: private
    │ └─ [3] ⚙️ FUNCTION: ERC4626Upgradeable.maxRedeem(address) (NodeID: 46)
    │     💬 Args: [owner]
    │     👁️  Def: public
    │   └─ [4] ⚙️ FUNCTION: ERC20Upgradeable.balanceOf(address) (NodeID: 47)
    │       💬 Args: [owner]
    │       👁️  Def: public
    │     └─ [5] ⚙️ FUNCTION: ERC20Upgradeable._getERC20Storage() (NodeID: 48)
    │         💬 Args: [no args]
    │         👁️  Def: private
    └─ [2] ⚙️ FUNCTION: Math.ternary(bool,uint256,uint256) (NodeID: 38)
        💬 Args: [a < b, a, b]
        👁️  Def: internal
      └─ [3] ⚙️ FUNCTION: SafeCast.toUint(bool) (NodeID: 39)
          💬 Args: [condition]
          👁️  Def: internal
```

## Documentation

### Function Documentation

@inheritdoc ERC4626Upgradeable
 @dev Limited by both owner's balance and Aave pool liquidity

### Interface Documentation

 @dev Returns the maximum amount of Vault shares that can be redeemed from the owner balance in the Vault,
 through a redeem call.
 - MUST return a limited value if owner is subject to some withdrawal limit or timelock.
 - MUST return balanceOf(owner) if owner is not subject to any withdrawal limit or timelock.
 - MUST NOT revert.
