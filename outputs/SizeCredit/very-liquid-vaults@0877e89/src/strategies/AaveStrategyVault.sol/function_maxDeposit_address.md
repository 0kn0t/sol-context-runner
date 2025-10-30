# Function: maxDeposit(address)

**Contract**: [src/strategies/AaveStrategyVault.sol/contract_AaveStrategyVault.md]

## Metadata

- **Contract**: AaveStrategyVault
- **Signature**: `maxDeposit(address)`
- **Visibility**: public
- **Source Range**: 4222:976:146

## Implementation

```solidity
/// @inheritdoc ERC4626Upgradeable
///  @dev Checks Aave reserve configuration and supply cap to determine max deposit
///  @dev Updates Superform implementation to comply with https://github.com/aave-dao/aave-v3-origin/blob/v3.4.0/src/contracts/protocol/libraries/logic/ValidationLogic.sol#L79-L85
///  @return The maximum deposit amount allowed by Aave
function maxDeposit(address receiver) override(BaseVault) public view returns (uint256) {
    DataTypes.ReserveConfigurationMap memory config = pool().getReserveData(asset()).configuration;
    if (!((config.getActive() && (!config.getFrozen())) && (!config.getPaused()))) return 0;
    uint256 supplyCapInWholeTokens = config.getSupplyCap();
    if (supplyCapInWholeTokens == 0) return super.maxDeposit(receiver);
    uint256 tokenDecimals = config.getDecimals();
    uint256 supplyCap = supplyCapInWholeTokens * (10 ** tokenDecimals);
    DataTypes.ReserveDataLegacy memory reserve = pool().getReserveData(asset());
    uint256 usedSupply = (aToken().scaledTotalSupply() + uint256(reserve.accruedToTreasury)).rayMul(reserve.liquidityIndex);
    if (usedSupply >= supplyCap) return 0;
    return Math.min(supplyCap - usedSupply, super.maxDeposit(receiver));
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

### getFrozen(struct DataTypes.ReserveConfigurationMap)

- **Kind**: internal
- **Source**: 9582:143:27
- **Link**: `lib/aave-v3-origin/src/contracts/protocol/libraries/configuration/ReserveConfiguration.sol:ReserveConfiguration:getFrozen(struct DataTypes.ReserveConfigurationMap)`

```solidity
///  @notice Gets the frozen state of the reserve
///  @param self The reserve configuration
///  @return The frozen state
function getFrozen(DataTypes.ReserveConfigurationMap memory self) internal pure returns (bool) {
    return (self.data & FROZEN_MASK) != 0;
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

### getSupplyCap(struct DataTypes.ReserveConfigurationMap)

- **Kind**: internal
- **Source**: 15801:189:27
- **Link**: `lib/aave-v3-origin/src/contracts/protocol/libraries/configuration/ReserveConfiguration.sol:ReserveConfiguration:getSupplyCap(struct DataTypes.ReserveConfigurationMap)`

```solidity
///  @notice Gets the supply cap of the reserve
///  @param self The reserve configuration
///  @return The supply cap
function getSupplyCap(DataTypes.ReserveConfigurationMap memory self) internal pure returns (uint256) {
    return (self.data & SUPPLY_CAP_MASK) >> SUPPLY_CAP_START_BIT_POSITION;
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

### getDecimals(struct DataTypes.ReserveConfigurationMap)

- **Kind**: internal
- **Source**: 8251:192:27
- **Link**: `lib/aave-v3-origin/src/contracts/protocol/libraries/configuration/ReserveConfiguration.sol:ReserveConfiguration:getDecimals(struct DataTypes.ReserveConfigurationMap)`

```solidity
///  @notice Gets the decimals of the underlying asset of the reserve
///  @param self The reserve configuration
///  @return The decimals of the asset
function getDecimals(DataTypes.ReserveConfigurationMap memory self) internal pure returns (uint256) {
    return (self.data & DECIMALS_MASK) >> RESERVE_DECIMALS_START_BIT_POSITION;
}
```

### rayMul(uint256,uint256)

- **Kind**: internal
- **Source**: 2253:319:29
- **Link**: `lib/aave-v3-origin/src/contracts/protocol/libraries/math/WadRayMath.sol:WadRayMath:rayMul(uint256,uint256)`

```solidity
///  @notice Multiplies two ray, rounding half up to the nearest ray
///  @dev assembly optimized for improved gas savings, see https://twitter.com/transmissions11/status/1451131036377571328
///  @param a Ray
///  @param b Ray
///  @return c = a raymul b
function rayMul(uint256 a, uint256 b) internal pure returns (uint256 c) {
    assembly {
        if iszero(or(iszero(b), iszero(gt(a, div(sub(not(0), HALF_RAY), b))))) {
            revert(0, 0)
        }
        c := div(add(mul(a, b), HALF_RAY), RAY)
    }
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

## External Calls

- **IPool::getReserveData(address)**
- **IAToken::scaledTotalSupply()**

## State Variable Reads

- **PAUSED_MASK** (`uint256`)
- **FROZEN_MASK** (`uint256`)
- **ACTIVE_MASK** (`uint256`)
- **SUPPLY_CAP_MASK** (`uint256`)
- **SUPPLY_CAP_START_BIT_POSITION** (`uint256`)
- **DECIMALS_MASK** (`uint256`)
- **RESERVE_DECIMALS_START_BIT_POSITION** (`uint256`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: AaveStrategyVault.maxDeposit(address) (NodeID: 0)
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
  ├─ [1] ⚙️ FUNCTION: ReserveConfiguration.getFrozen(struct DataTypes.ReserveConfigurationMap) (NodeID: 6)
  │   💬 Args: [config]
  │   👁️  Def: internal
  ├─ [1] ⚙️ FUNCTION: ReserveConfiguration.getActive(struct DataTypes.ReserveConfigurationMap) (NodeID: 7)
  │   💬 Args: [config]
  │   👁️  Def: internal
  ├─ [1] ⚙️ FUNCTION: ReserveConfiguration.getSupplyCap(struct DataTypes.ReserveConfigurationMap) (NodeID: 8)
  │   💬 Args: [config]
  │   👁️  Def: internal
  ├─ [1] ⚙️ FUNCTION: BaseVault.maxDeposit(address) (NodeID: 9)
  │   💬 Args: [receiver]
  │   👁️  Def: public
  │ ├─ [2] ⚙️ FUNCTION: BaseVault._pausedOrAuthPaused() (NodeID: 10)
  │ │   💬 Args: [no args]
  │ │   👁️  Def: private
  │ │ ├─ [3] ⚙️ FUNCTION: BaseVault.auth() (NodeID: 11)
  │ │ │   💬 Args: [no args]
  │ │ │   👁️  Def: public
  │ │ │ └─ [4] ⚙️ FUNCTION: BaseVault._getBaseVaultStorage() (NodeID: 12)
  │ │ │     💬 Args: [no args]
  │ │ │     👁️  Def: private
  │ │ └─ [3] ⚙️ FUNCTION: PausableUpgradeable.paused() (NodeID: 13)
  │ │     💬 Args: [no args]
  │ │     👁️  Def: public
  │ │   └─ [4] ⚙️ FUNCTION: PausableUpgradeable._getPausableStorage() (NodeID: 14)
  │ │       💬 Args: [no args]
  │ │       👁️  Def: private
  │ ├─ [2] ⚙️ FUNCTION: BaseVault._totalAssetsCap() (NodeID: 15)
  │ │   💬 Args: [no args]
  │ │   👁️  Def: private
  │ │ └─ [3] ⚙️ FUNCTION: BaseVault._getBaseVaultStorage() (NodeID: 16)
  │ │     💬 Args: [no args]
  │ │     👁️  Def: private
  │ ├─ [2] ⚙️ FUNCTION: ERC4626Upgradeable.maxDeposit(address) (NodeID: 17)
  │ │   💬 Args: [receiver]
  │ │   👁️  Def: public
  │ └─ [2] ⚙️ FUNCTION: BaseVault._maxDeposit() (NodeID: 18)
  │     💬 Args: [no args]
  │     👁️  Def: private
  │   └─ [3] ⚙️ FUNCTION: Math.saturatingSub(uint256,uint256) (NodeID: 19)
  │       💬 Args: [_totalAssetsCap(), totalAssets()]
  │       👁️  Def: internal
  │     ├─ [4] ⚙️ FUNCTION: BaseVault._totalAssetsCap() (NodeID: 22)
  │     │   💬 Args: [no args]
  │     │   👁️  Def: private
  │     │ └─ [5] ⚙️ FUNCTION: BaseVault._getBaseVaultStorage() (NodeID: 23)
  │     │     💬 Args: [no args]
  │     │     👁️  Def: private
  │     ├─ [4] ⚙️ FUNCTION: AaveStrategyVault.totalAssets() (NodeID: 24)
  │     │   💬 Args: [no args]
  │     │   👁️  Def: public
  │     │ ├─ [5] ⚙️ FUNCTION: AaveStrategyVault.pool() (NodeID: 25)
  │     │ │   💬 Args: [no args]
  │     │ │   👁️  Def: public
  │     │ │ └─ [6] ⚙️ FUNCTION: AaveStrategyVault._getAaveStrategyVaultStorage() (NodeID: 26)
  │     │ │     💬 Args: [no args]
  │     │ │     👁️  Def: private
  │     │ ├─ [5] ⚙️ FUNCTION: ERC4626Upgradeable.asset() (NodeID: 27)
  │     │ │   💬 Args: [no args]
  │     │ │   👁️  Def: public
  │     │ │ └─ [6] ⚙️ FUNCTION: ERC4626Upgradeable._getERC4626Storage() (NodeID: 28)
  │     │ │     💬 Args: [no args]
  │     │ │     👁️  Def: private
  │     │ └─ [5] ⚙️ FUNCTION: Math.mulDiv(uint256,uint256,uint256) (NodeID: 29)
  │     │     💬 Args: [aToken().scaledBalanceOf(address(this)), liquidityIndex, WadRayMath.RAY]
  │     │     👁️  Def: internal
  │     │   ├─ [6] ⚙️ FUNCTION: AaveStrategyVault.aToken() (NodeID: 34)
  │     │   │   💬 Args: [no args]
  │     │   │   👁️  Def: public
  │     │   │ └─ [7] ⚙️ FUNCTION: AaveStrategyVault._getAaveStrategyVaultStorage() (NodeID: 35)
  │     │   │     💬 Args: [no args]
  │     │   │     👁️  Def: private
  │     │   ├─ [6] ⚙️ FUNCTION: Math.mul512(uint256,uint256) (NodeID: 30)
  │     │   │   💬 Args: [x, y]
  │     │   │   👁️  Def: internal
  │     │   └─ [6] ⚙️ FUNCTION: Panic.panic(uint256) (NodeID: 31)
  │     │       💬 Args: [ternary(denominator == 0, Panic.DIVISION_BY_ZERO, Panic.UNDER_OVERFLOW)]
  │     │       👁️  Def: internal
  │     │     └─ [7] ⚙️ FUNCTION: Math.ternary(bool,uint256,uint256) (NodeID: 32)
  │     │         💬 Args: [denominator == 0, Panic.DIVISION_BY_ZERO, Panic.UNDER_OVERFLOW]
  │     │         👁️  Def: internal
  │     │       └─ [8] ⚙️ FUNCTION: SafeCast.toUint(bool) (NodeID: 33)
  │     │           💬 Args: [condition]
  │     │           👁️  Def: internal
  │     └─ [4] ⚙️ FUNCTION: Math.trySub(uint256,uint256) (NodeID: 20)
  │         💬 Args: [a, b]
  │         👁️  Def: internal
  │       └─ [5] ⚙️ FUNCTION: SafeCast.toUint(bool) (NodeID: 21)
  │           💬 Args: [success]
  │           👁️  Def: internal
  ├─ [1] ⚙️ FUNCTION: ReserveConfiguration.getDecimals(struct DataTypes.ReserveConfigurationMap) (NodeID: 36)
  │   💬 Args: [config]
  │   👁️  Def: internal
  ├─ [1] ⚙️ FUNCTION: AaveStrategyVault.pool() (NodeID: 37)
  │   💬 Args: [no args]
  │   👁️  Def: public
  │ └─ [2] ⚙️ FUNCTION: AaveStrategyVault._getAaveStrategyVaultStorage() (NodeID: 38)
  │     💬 Args: [no args]
  │     👁️  Def: private
  ├─ [1] ⚙️ FUNCTION: ERC4626Upgradeable.asset() (NodeID: 39)
  │   💬 Args: [no args]
  │   👁️  Def: public
  │ └─ [2] ⚙️ FUNCTION: ERC4626Upgradeable._getERC4626Storage() (NodeID: 40)
  │     💬 Args: [no args]
  │     👁️  Def: private
  ├─ [1] ⚙️ FUNCTION: AaveStrategyVault.aToken() (NodeID: 41)
  │   💬 Args: [no args]
  │   👁️  Def: public
  │ └─ [2] ⚙️ FUNCTION: AaveStrategyVault._getAaveStrategyVaultStorage() (NodeID: 42)
  │     💬 Args: [no args]
  │     👁️  Def: private
  ├─ [1] ⚙️ FUNCTION: WadRayMath.rayMul(uint256,uint256) (NodeID: 43)
  │   💬 Args: [(aToken().scaledTotalSupply() + uint256(reserve.accruedToTreasury)), reserve.liquidityIndex]
  │   👁️  Def: internal
  └─ [1] ⚙️ FUNCTION: Math.min(uint256,uint256) (NodeID: 44)
      💬 Args: [supplyCap - usedSupply, super.maxDeposit(receiver)]
      👁️  Def: internal
    ├─ [2] ⚙️ FUNCTION: BaseVault.maxDeposit(address) (NodeID: 47)
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
    │ ├─ [3] ⚙️ FUNCTION: ERC4626Upgradeable.maxDeposit(address) (NodeID: 55)
    │ │   💬 Args: [receiver]
    │ │   👁️  Def: public
    │ └─ [3] ⚙️ FUNCTION: BaseVault._maxDeposit() (NodeID: 56)
    │     💬 Args: [no args]
    │     👁️  Def: private
    │   └─ [4] ⚙️ FUNCTION: Math.saturatingSub(uint256,uint256) (NodeID: 57)
    │       💬 Args: [_totalAssetsCap(), totalAssets()]
    │       👁️  Def: internal
    │     ├─ [5] ⚙️ FUNCTION: BaseVault._totalAssetsCap() (NodeID: 60)
    │     │   💬 Args: [no args]
    │     │   👁️  Def: private
    │     │ └─ [6] ⚙️ FUNCTION: BaseVault._getBaseVaultStorage() (NodeID: 61)
    │     │     💬 Args: [no args]
    │     │     👁️  Def: private
    │     ├─ [5] ⚙️ FUNCTION: AaveStrategyVault.totalAssets() (NodeID: 62)
    │     │   💬 Args: [no args]
    │     │   👁️  Def: public
    │     │ ├─ [6] ⚙️ FUNCTION: AaveStrategyVault.pool() (NodeID: 63)
    │     │ │   💬 Args: [no args]
    │     │ │   👁️  Def: public
    │     │ │ └─ [7] ⚙️ FUNCTION: AaveStrategyVault._getAaveStrategyVaultStorage() (NodeID: 64)
    │     │ │     💬 Args: [no args]
    │     │ │     👁️  Def: private
    │     │ ├─ [6] ⚙️ FUNCTION: ERC4626Upgradeable.asset() (NodeID: 65)
    │     │ │   💬 Args: [no args]
    │     │ │   👁️  Def: public
    │     │ │ └─ [7] ⚙️ FUNCTION: ERC4626Upgradeable._getERC4626Storage() (NodeID: 66)
    │     │ │     💬 Args: [no args]
    │     │ │     👁️  Def: private
    │     │ └─ [6] ⚙️ FUNCTION: Math.mulDiv(uint256,uint256,uint256) (NodeID: 67)
    │     │     💬 Args: [aToken().scaledBalanceOf(address(this)), liquidityIndex, WadRayMath.RAY]
    │     │     👁️  Def: internal
    │     │   ├─ [7] ⚙️ FUNCTION: AaveStrategyVault.aToken() (NodeID: 72)
    │     │   │   💬 Args: [no args]
    │     │   │   👁️  Def: public
    │     │   │ └─ [8] ⚙️ FUNCTION: AaveStrategyVault._getAaveStrategyVaultStorage() (NodeID: 73)
    │     │   │     💬 Args: [no args]
    │     │   │     👁️  Def: private
    │     │   ├─ [7] ⚙️ FUNCTION: Math.mul512(uint256,uint256) (NodeID: 68)
    │     │   │   💬 Args: [x, y]
    │     │   │   👁️  Def: internal
    │     │   └─ [7] ⚙️ FUNCTION: Panic.panic(uint256) (NodeID: 69)
    │     │       💬 Args: [ternary(denominator == 0, Panic.DIVISION_BY_ZERO, Panic.UNDER_OVERFLOW)]
    │     │       👁️  Def: internal
    │     │     └─ [8] ⚙️ FUNCTION: Math.ternary(bool,uint256,uint256) (NodeID: 70)
    │     │         💬 Args: [denominator == 0, Panic.DIVISION_BY_ZERO, Panic.UNDER_OVERFLOW]
    │     │         👁️  Def: internal
    │     │       └─ [9] ⚙️ FUNCTION: SafeCast.toUint(bool) (NodeID: 71)
    │     │           💬 Args: [condition]
    │     │           👁️  Def: internal
    │     └─ [5] ⚙️ FUNCTION: Math.trySub(uint256,uint256) (NodeID: 58)
    │         💬 Args: [a, b]
    │         👁️  Def: internal
    │       └─ [6] ⚙️ FUNCTION: SafeCast.toUint(bool) (NodeID: 59)
    │           💬 Args: [success]
    │           👁️  Def: internal
    └─ [2] ⚙️ FUNCTION: Math.ternary(bool,uint256,uint256) (NodeID: 45)
        💬 Args: [a < b, a, b]
        👁️  Def: internal
      └─ [3] ⚙️ FUNCTION: SafeCast.toUint(bool) (NodeID: 46)
          💬 Args: [condition]
          👁️  Def: internal
```

## Documentation

### Function Documentation

@inheritdoc ERC4626Upgradeable
 @dev Checks Aave reserve configuration and supply cap to determine max deposit
 @dev Updates Superform implementation to comply with https://github.com/aave-dao/aave-v3-origin/blob/v3.4.0/src/contracts/protocol/libraries/logic/ValidationLogic.sol#L79-L85
 @return The maximum deposit amount allowed by Aave

### Interface Documentation

 @dev Returns the maximum amount of the underlying asset that can be deposited into the Vault for the receiver,
 through a deposit call.
 - MUST return a limited value if receiver is subject to some deposit limit.
 - MUST return 2 ** 256 - 1 if there is no limit on the maximum amount of assets that may be deposited.
 - MUST NOT revert.
