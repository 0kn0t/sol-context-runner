# Function: maxMint(address)

**Contract**: [src/strategies/AaveStrategyVault.sol/contract_AaveStrategyVault.md]

## Metadata

- **Contract**: AaveStrategyVault
- **Signature**: `maxMint(address)`
- **Visibility**: public
- **Source Range**: 5490:181:60

## Implementation

```solidity
/// @notice Returns the maximum number of shares that can be minted
///  @dev Converts the max deposit amount to shares
function maxMint(address receiver) override(BaseVault) public view returns (uint256) {
    return Math.min(convertToShares(maxDeposit(receiver)), super.maxMint(receiver));
}
```

## Related Implementations

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

### maxDeposit(address)

- **Kind**: internal
- **Source**: 4348:1009:60
- **Link**: `src/strategies/AaveStrategyVault.sol:AaveStrategyVault:maxDeposit(address)`

```solidity
/// @notice Returns the maximum amount that can be deposited
///  @dev Checks Aave reserve configuration and supply cap to determine max deposit
///  @dev Updates Superform implementation to comply with https://github.com/aave-dao/aave-v3-origin/blob/v3.4.0/src/contracts/protocol/libraries/logic/ValidationLogic.sol#L79-L85
///  @return The maximum deposit amount allowed by Aave
function maxDeposit(address receiver) override(BaseVault) public view returns (uint256) {
    DataTypes.ReserveConfigurationMap memory config = pool.getReserveData(asset()).configuration;
    if (!((config.getActive() && (!config.getFrozen())) && (!config.getPaused()))) {
        return 0;
    }
    uint256 supplyCapInWholeTokens = config.getSupplyCap();
    if (supplyCapInWholeTokens == 0) {
        return type(uint256).max;
    }
    uint256 tokenDecimals = config.getDecimals();
    uint256 supplyCap = supplyCapInWholeTokens * (10 ** tokenDecimals);
    DataTypes.ReserveDataLegacy memory reserve = pool.getReserveData(asset());
    uint256 usedSupply = (aToken.scaledTotalSupply() + uint256(reserve.accruedToTreasury)).rayMul(reserve.liquidityIndex);
    if (usedSupply >= supplyCap) return 0;
    return Math.min(supplyCap - usedSupply, super.maxDeposit(receiver));
}
```

### asset()

- **Kind**: internal
- **Source**: 6882:153:17
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
- **Source**: 4088:159:17
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
- **Source**: 10223:143:7
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
- **Source**: 9582:143:7
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
- **Source**: 8941:143:7
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
- **Source**: 15801:189:7
- **Link**: `lib/aave-v3-origin/src/contracts/protocol/libraries/configuration/ReserveConfiguration.sol:ReserveConfiguration:getSupplyCap(struct DataTypes.ReserveConfigurationMap)`

```solidity
///  @notice Gets the supply cap of the reserve
///  @param self The reserve configuration
///  @return The supply cap
function getSupplyCap(DataTypes.ReserveConfigurationMap memory self) internal pure returns (uint256) {
    return (self.data & SUPPLY_CAP_MASK) >> SUPPLY_CAP_START_BIT_POSITION;
}
```

### getDecimals(struct DataTypes.ReserveConfigurationMap)

- **Kind**: internal
- **Source**: 8251:192:7
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
- **Source**: 2253:319:9
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
- **Source**: 7481:398:60
- **Link**: `src/strategies/AaveStrategyVault.sol:AaveStrategyVault:totalAssets()`

```solidity
/// @notice Returns the total assets managed by this strategy
///  @dev Returns the aToken balance since aTokens represent the underlying asset with accrued interest
///  @dev Round down to avoid stealing assets in roundtrip operations https://github.com/a16z/erc4626-tests/issues/13
function totalAssets() virtual override(ERC4626Upgradeable, IERC4626) public view returns (uint256) {
    /// @notice aTokens use rebasing to accrue interest, so the total assets is just the aToken balance
    uint256 liquidityIndex = pool.getReserveNormalizedIncome(address(asset()));
    return Math.mulDiv(aToken.scaledBalanceOf(address(this)), liquidityIndex, WadRayMath.RAY);
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

- **pool** (`contract IPool`) [lib/aave-v3-origin/src/contracts/interfaces/IPool.sol/interface_IPool.md]
- **aToken** (`contract IAToken`) [lib/aave-v3-origin/src/contracts/interfaces/IAToken.sol/interface_IAToken.md]
- **PAUSED_MASK** (`uint256`)
- **FROZEN_MASK** (`uint256`)
- **ACTIVE_MASK** (`uint256`)
- **SUPPLY_CAP_MASK** (`uint256`)
- **SUPPLY_CAP_START_BIT_POSITION** (`uint256`)
- **DECIMALS_MASK** (`uint256`)
- **RESERVE_DECIMALS_START_BIT_POSITION** (`uint256`)
- **totalAssetsCap** (`uint256`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: AaveStrategyVault.maxMint(address) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
  └─ [1] ⚙️ FUNCTION: Math.min(uint256,uint256) (NodeID: 1)
      💬 Args: [convertToShares(maxDeposit(receiver)), super.maxMint(receiver)]
      👁️  Def: internal
    ├─ [2] ⚙️ FUNCTION: ERC4626Upgradeable.convertToShares(uint256) (NodeID: 4)
    │   💬 Args: [maxDeposit(receiver)]
    │   👁️  Def: public
    │ ├─ [3] ⚙️ FUNCTION: AaveStrategyVault.maxDeposit(address) (NodeID: 25)
    │ │   💬 Args: [receiver]
    │ │   👁️  Def: public
    │ │ ├─ [4] ⚙️ FUNCTION: ERC4626Upgradeable.asset() (NodeID: 26)
    │ │ │   💬 Args: [no args]
    │ │ │   👁️  Def: public
    │ │ │ └─ [5] ⚙️ FUNCTION: ERC4626Upgradeable._getERC4626Storage() (NodeID: 27)
    │ │ │     💬 Args: [no args]
    │ │ │     👁️  Def: private
    │ │ ├─ [4] ⚙️ FUNCTION: ReserveConfiguration.getPaused(struct DataTypes.ReserveConfigurationMap) (NodeID: 28)
    │ │ │   💬 Args: [config]
    │ │ │   👁️  Def: internal
    │ │ ├─ [4] ⚙️ FUNCTION: ReserveConfiguration.getFrozen(struct DataTypes.ReserveConfigurationMap) (NodeID: 29)
    │ │ │   💬 Args: [config]
    │ │ │   👁️  Def: internal
    │ │ ├─ [4] ⚙️ FUNCTION: ReserveConfiguration.getActive(struct DataTypes.ReserveConfigurationMap) (NodeID: 30)
    │ │ │   💬 Args: [config]
    │ │ │   👁️  Def: internal
    │ │ ├─ [4] ⚙️ FUNCTION: ReserveConfiguration.getSupplyCap(struct DataTypes.ReserveConfigurationMap) (NodeID: 31)
    │ │ │   💬 Args: [config]
    │ │ │   👁️  Def: internal
    │ │ ├─ [4] ⚙️ FUNCTION: ReserveConfiguration.getDecimals(struct DataTypes.ReserveConfigurationMap) (NodeID: 32)
    │ │ │   💬 Args: [config]
    │ │ │   👁️  Def: internal
    │ │ ├─ [4] ⚙️ FUNCTION: ERC4626Upgradeable.asset() (NodeID: 33)
    │ │ │   💬 Args: [no args]
    │ │ │   👁️  Def: public
    │ │ │ └─ [5] ⚙️ FUNCTION: ERC4626Upgradeable._getERC4626Storage() (NodeID: 34)
    │ │ │     💬 Args: [no args]
    │ │ │     👁️  Def: private
    │ │ ├─ [4] ⚙️ FUNCTION: WadRayMath.rayMul(uint256,uint256) (NodeID: 35)
    │ │ │   💬 Args: [(aToken.scaledTotalSupply() + uint256(reserve.accruedToTreasury)), reserve.liquidityIndex]
    │ │ │   👁️  Def: internal
    │ │ └─ [4] ⚙️ FUNCTION: Math.min(uint256,uint256) (NodeID: 36)
    │ │     💬 Args: [supplyCap - usedSupply, super.maxDeposit(receiver)]
    │ │     👁️  Def: internal
    │ │   ├─ [5] ⚙️ FUNCTION: BaseVault.maxDeposit(address) (NodeID: 39)
    │ │   │   💬 Args: [receiver]
    │ │   │   👁️  Def: public
    │ │   │ └─ [6] ⚙️ FUNCTION: Math.saturatingSub(uint256,uint256) (NodeID: 40)
    │ │   │     💬 Args: [totalAssetsCap, totalAssets()]
    │ │   │     👁️  Def: internal
    │ │   │   ├─ [7] ⚙️ FUNCTION: AaveStrategyVault.totalAssets() (NodeID: 43)
    │ │   │   │   💬 Args: [no args]
    │ │   │   │   👁️  Def: public
    │ │   │   │ ├─ [8] ⚙️ FUNCTION: ERC4626Upgradeable.asset() (NodeID: 44)
    │ │   │   │ │   💬 Args: [no args]
    │ │   │   │ │   👁️  Def: public
    │ │   │   │ │ └─ [9] ⚙️ FUNCTION: ERC4626Upgradeable._getERC4626Storage() (NodeID: 45)
    │ │   │   │ │     💬 Args: [no args]
    │ │   │   │ │     👁️  Def: private
    │ │   │   │ └─ [8] ⚙️ FUNCTION: Math.mulDiv(uint256,uint256,uint256) (NodeID: 46)
    │ │   │   │     💬 Args: [aToken.scaledBalanceOf(address(this)), liquidityIndex, WadRayMath.RAY]
    │ │   │   │     👁️  Def: internal
    │ │   │   │   ├─ [9] ⚙️ FUNCTION: Math.mul512(uint256,uint256) (NodeID: 47)
    │ │   │   │   │   💬 Args: [x, y]
    │ │   │   │   │   👁️  Def: internal
    │ │   │   │   └─ [9] ⚙️ FUNCTION: Panic.panic(uint256) (NodeID: 48)
    │ │   │   │       💬 Args: [ternary(denominator == 0, Panic.DIVISION_BY_ZERO, Panic.UNDER_OVERFLOW)]
    │ │   │   │       👁️  Def: internal
    │ │   │   │     └─ [10] ⚙️ FUNCTION: Math.ternary(bool,uint256,uint256) (NodeID: 49)
    │ │   │   │         💬 Args: [denominator == 0, Panic.DIVISION_BY_ZERO, Panic.UNDER_OVERFLOW]
    │ │   │   │         👁️  Def: internal
    │ │   │   │       └─ [11] ⚙️ FUNCTION: SafeCast.toUint(bool) (NodeID: 50)
    │ │   │   │           💬 Args: [condition]
    │ │   │   │           👁️  Def: internal
    │ │   │   └─ [7] ⚙️ FUNCTION: Math.trySub(uint256,uint256) (NodeID: 41)
    │ │   │       💬 Args: [a, b]
    │ │   │       👁️  Def: internal
    │ │   │     └─ [8] ⚙️ FUNCTION: SafeCast.toUint(bool) (NodeID: 42)
    │ │   │         💬 Args: [success]
    │ │   │         👁️  Def: internal
    │ │   └─ [5] ⚙️ FUNCTION: Math.ternary(bool,uint256,uint256) (NodeID: 37)
    │ │       💬 Args: [a < b, a, b]
    │ │       👁️  Def: internal
    │ │     └─ [6] ⚙️ FUNCTION: SafeCast.toUint(bool) (NodeID: 38)
    │ │         💬 Args: [condition]
    │ │         👁️  Def: internal
    │ └─ [3] ⚙️ FUNCTION: ERC4626Upgradeable._convertToShares(uint256,enum Math.Rounding) (NodeID: 5)
    │     💬 Args: [assets, Math.Rounding.Floor]
    │     👁️  Def: internal
    │   └─ [4] ⚙️ FUNCTION: Math.mulDiv(uint256,uint256,uint256,enum Math.Rounding) (NodeID: 6)
    │       💬 Args: [assets, totalSupply() + (10 ** _decimalsOffset()), totalAssets() + 1, rounding]
    │       👁️  Def: internal
    │     ├─ [5] ⚙️ FUNCTION: ERC4626Upgradeable._decimalsOffset() (NodeID: 14)
    │     │   💬 Args: [no args]
    │     │   👁️  Def: internal
    │     ├─ [5] ⚙️ FUNCTION: ERC20Upgradeable.totalSupply() (NodeID: 15)
    │     │   💬 Args: [no args]
    │     │   👁️  Def: public
    │     │ └─ [6] ⚙️ FUNCTION: ERC20Upgradeable._getERC20Storage() (NodeID: 16)
    │     │     💬 Args: [no args]
    │     │     👁️  Def: private
    │     ├─ [5] ⚙️ FUNCTION: AaveStrategyVault.totalAssets() (NodeID: 17)
    │     │   💬 Args: [no args]
    │     │   👁️  Def: public
    │     │ ├─ [6] ⚙️ FUNCTION: ERC4626Upgradeable.asset() (NodeID: 18)
    │     │ │   💬 Args: [no args]
    │     │ │   👁️  Def: public
    │     │ │ └─ [7] ⚙️ FUNCTION: ERC4626Upgradeable._getERC4626Storage() (NodeID: 19)
    │     │ │     💬 Args: [no args]
    │     │ │     👁️  Def: private
    │     │ └─ [6] ⚙️ FUNCTION: Math.mulDiv(uint256,uint256,uint256) (NodeID: 20)
    │     │     💬 Args: [aToken.scaledBalanceOf(address(this)), liquidityIndex, WadRayMath.RAY]
    │     │     👁️  Def: internal
    │     │   ├─ [7] ⚙️ FUNCTION: Math.mul512(uint256,uint256) (NodeID: 21)
    │     │   │   💬 Args: [x, y]
    │     │   │   👁️  Def: internal
    │     │   └─ [7] ⚙️ FUNCTION: Panic.panic(uint256) (NodeID: 22)
    │     │       💬 Args: [ternary(denominator == 0, Panic.DIVISION_BY_ZERO, Panic.UNDER_OVERFLOW)]
    │     │       👁️  Def: internal
    │     │     └─ [8] ⚙️ FUNCTION: Math.ternary(bool,uint256,uint256) (NodeID: 23)
    │     │         💬 Args: [denominator == 0, Panic.DIVISION_BY_ZERO, Panic.UNDER_OVERFLOW]
    │     │         👁️  Def: internal
    │     │       └─ [9] ⚙️ FUNCTION: SafeCast.toUint(bool) (NodeID: 24)
    │     │           💬 Args: [condition]
    │     │           👁️  Def: internal
    │     ├─ [5] ⚙️ FUNCTION: SafeCast.toUint(bool) (NodeID: 7)
    │     │   💬 Args: [unsignedRoundsUp(rounding) && (mulmod(x, y, denominator) > 0)]
    │     │   👁️  Def: internal
    │     │ └─ [6] ⚙️ FUNCTION: Math.unsignedRoundsUp(enum Math.Rounding) (NodeID: 8)
    │     │     💬 Args: [rounding]
    │     │     👁️  Def: internal
    │     └─ [5] ⚙️ FUNCTION: Math.mulDiv(uint256,uint256,uint256) (NodeID: 9)
    │         💬 Args: [x, y, denominator]
    │         👁️  Def: internal
    │       ├─ [6] ⚙️ FUNCTION: Math.mul512(uint256,uint256) (NodeID: 10)
    │       │   💬 Args: [x, y]
    │       │   👁️  Def: internal
    │       └─ [6] ⚙️ FUNCTION: Panic.panic(uint256) (NodeID: 11)
    │           💬 Args: [ternary(denominator == 0, Panic.DIVISION_BY_ZERO, Panic.UNDER_OVERFLOW)]
    │           👁️  Def: internal
    │         └─ [7] ⚙️ FUNCTION: Math.ternary(bool,uint256,uint256) (NodeID: 12)
    │             💬 Args: [denominator == 0, Panic.DIVISION_BY_ZERO, Panic.UNDER_OVERFLOW]
    │             👁️  Def: internal
    │           └─ [8] ⚙️ FUNCTION: SafeCast.toUint(bool) (NodeID: 13)
    │               💬 Args: [condition]
    │               👁️  Def: internal
    ├─ [2] ⚙️ FUNCTION: BaseVault.maxMint(address) (NodeID: 51)
    │   💬 Args: [receiver]
    │   👁️  Def: public
    │ └─ [3] ⚙️ FUNCTION: ERC4626Upgradeable.convertToShares(uint256) (NodeID: 52)
    │     💬 Args: [maxDeposit(receiver)]
    │     👁️  Def: public
    │   ├─ [4] ⚙️ FUNCTION: AaveStrategyVault.maxDeposit(address) (NodeID: 73)
    │   │   💬 Args: [receiver]
    │   │   👁️  Def: public
    │   │ ├─ [5] ⚙️ FUNCTION: ERC4626Upgradeable.asset() (NodeID: 74)
    │   │ │   💬 Args: [no args]
    │   │ │   👁️  Def: public
    │   │ │ └─ [6] ⚙️ FUNCTION: ERC4626Upgradeable._getERC4626Storage() (NodeID: 75)
    │   │ │     💬 Args: [no args]
    │   │ │     👁️  Def: private
    │   │ ├─ [5] ⚙️ FUNCTION: ReserveConfiguration.getPaused(struct DataTypes.ReserveConfigurationMap) (NodeID: 76)
    │   │ │   💬 Args: [config]
    │   │ │   👁️  Def: internal
    │   │ ├─ [5] ⚙️ FUNCTION: ReserveConfiguration.getFrozen(struct DataTypes.ReserveConfigurationMap) (NodeID: 77)
    │   │ │   💬 Args: [config]
    │   │ │   👁️  Def: internal
    │   │ ├─ [5] ⚙️ FUNCTION: ReserveConfiguration.getActive(struct DataTypes.ReserveConfigurationMap) (NodeID: 78)
    │   │ │   💬 Args: [config]
    │   │ │   👁️  Def: internal
    │   │ ├─ [5] ⚙️ FUNCTION: ReserveConfiguration.getSupplyCap(struct DataTypes.ReserveConfigurationMap) (NodeID: 79)
    │   │ │   💬 Args: [config]
    │   │ │   👁️  Def: internal
    │   │ ├─ [5] ⚙️ FUNCTION: ReserveConfiguration.getDecimals(struct DataTypes.ReserveConfigurationMap) (NodeID: 80)
    │   │ │   💬 Args: [config]
    │   │ │   👁️  Def: internal
    │   │ ├─ [5] ⚙️ FUNCTION: ERC4626Upgradeable.asset() (NodeID: 81)
    │   │ │   💬 Args: [no args]
    │   │ │   👁️  Def: public
    │   │ │ └─ [6] ⚙️ FUNCTION: ERC4626Upgradeable._getERC4626Storage() (NodeID: 82)
    │   │ │     💬 Args: [no args]
    │   │ │     👁️  Def: private
    │   │ ├─ [5] ⚙️ FUNCTION: WadRayMath.rayMul(uint256,uint256) (NodeID: 83)
    │   │ │   💬 Args: [(aToken.scaledTotalSupply() + uint256(reserve.accruedToTreasury)), reserve.liquidityIndex]
    │   │ │   👁️  Def: internal
    │   │ └─ [5] ⚙️ FUNCTION: Math.min(uint256,uint256) (NodeID: 84)
    │   │     💬 Args: [supplyCap - usedSupply, super.maxDeposit(receiver)]
    │   │     👁️  Def: internal
    │   │   ├─ [6] ⚙️ FUNCTION: BaseVault.maxDeposit(address) (NodeID: 87)
    │   │   │   💬 Args: [receiver]
    │   │   │   👁️  Def: public
    │   │   │ └─ [7] ⚙️ FUNCTION: Math.saturatingSub(uint256,uint256) (NodeID: 88)
    │   │   │     💬 Args: [totalAssetsCap, totalAssets()]
    │   │   │     👁️  Def: internal
    │   │   │   ├─ [8] ⚙️ FUNCTION: AaveStrategyVault.totalAssets() (NodeID: 91)
    │   │   │   │   💬 Args: [no args]
    │   │   │   │   👁️  Def: public
    │   │   │   │ ├─ [9] ⚙️ FUNCTION: ERC4626Upgradeable.asset() (NodeID: 92)
    │   │   │   │ │   💬 Args: [no args]
    │   │   │   │ │   👁️  Def: public
    │   │   │   │ │ └─ [10] ⚙️ FUNCTION: ERC4626Upgradeable._getERC4626Storage() (NodeID: 93)
    │   │   │   │ │     💬 Args: [no args]
    │   │   │   │ │     👁️  Def: private
    │   │   │   │ └─ [9] ⚙️ FUNCTION: Math.mulDiv(uint256,uint256,uint256) (NodeID: 94)
    │   │   │   │     💬 Args: [aToken.scaledBalanceOf(address(this)), liquidityIndex, WadRayMath.RAY]
    │   │   │   │     👁️  Def: internal
    │   │   │   │   ├─ [10] ⚙️ FUNCTION: Math.mul512(uint256,uint256) (NodeID: 95)
    │   │   │   │   │   💬 Args: [x, y]
    │   │   │   │   │   👁️  Def: internal
    │   │   │   │   └─ [10] ⚙️ FUNCTION: Panic.panic(uint256) (NodeID: 96)
    │   │   │   │       💬 Args: [ternary(denominator == 0, Panic.DIVISION_BY_ZERO, Panic.UNDER_OVERFLOW)]
    │   │   │   │       👁️  Def: internal
    │   │   │   │     └─ [11] ⚙️ FUNCTION: Math.ternary(bool,uint256,uint256) (NodeID: 97)
    │   │   │   │         💬 Args: [denominator == 0, Panic.DIVISION_BY_ZERO, Panic.UNDER_OVERFLOW]
    │   │   │   │         👁️  Def: internal
    │   │   │   │       └─ [12] ⚙️ FUNCTION: SafeCast.toUint(bool) (NodeID: 98)
    │   │   │   │           💬 Args: [condition]
    │   │   │   │           👁️  Def: internal
    │   │   │   └─ [8] ⚙️ FUNCTION: Math.trySub(uint256,uint256) (NodeID: 89)
    │   │   │       💬 Args: [a, b]
    │   │   │       👁️  Def: internal
    │   │   │     └─ [9] ⚙️ FUNCTION: SafeCast.toUint(bool) (NodeID: 90)
    │   │   │         💬 Args: [success]
    │   │   │         👁️  Def: internal
    │   │   └─ [6] ⚙️ FUNCTION: Math.ternary(bool,uint256,uint256) (NodeID: 85)
    │   │       💬 Args: [a < b, a, b]
    │   │       👁️  Def: internal
    │   │     └─ [7] ⚙️ FUNCTION: SafeCast.toUint(bool) (NodeID: 86)
    │   │         💬 Args: [condition]
    │   │         👁️  Def: internal
    │   └─ [4] ⚙️ FUNCTION: ERC4626Upgradeable._convertToShares(uint256,enum Math.Rounding) (NodeID: 53)
    │       💬 Args: [assets, Math.Rounding.Floor]
    │       👁️  Def: internal
    │     └─ [5] ⚙️ FUNCTION: Math.mulDiv(uint256,uint256,uint256,enum Math.Rounding) (NodeID: 54)
    │         💬 Args: [assets, totalSupply() + (10 ** _decimalsOffset()), totalAssets() + 1, rounding]
    │         👁️  Def: internal
    │       ├─ [6] ⚙️ FUNCTION: ERC4626Upgradeable._decimalsOffset() (NodeID: 62)
    │       │   💬 Args: [no args]
    │       │   👁️  Def: internal
    │       ├─ [6] ⚙️ FUNCTION: ERC20Upgradeable.totalSupply() (NodeID: 63)
    │       │   💬 Args: [no args]
    │       │   👁️  Def: public
    │       │ └─ [7] ⚙️ FUNCTION: ERC20Upgradeable._getERC20Storage() (NodeID: 64)
    │       │     💬 Args: [no args]
    │       │     👁️  Def: private
    │       ├─ [6] ⚙️ FUNCTION: AaveStrategyVault.totalAssets() (NodeID: 65)
    │       │   💬 Args: [no args]
    │       │   👁️  Def: public
    │       │ ├─ [7] ⚙️ FUNCTION: ERC4626Upgradeable.asset() (NodeID: 66)
    │       │ │   💬 Args: [no args]
    │       │ │   👁️  Def: public
    │       │ │ └─ [8] ⚙️ FUNCTION: ERC4626Upgradeable._getERC4626Storage() (NodeID: 67)
    │       │ │     💬 Args: [no args]
    │       │ │     👁️  Def: private
    │       │ └─ [7] ⚙️ FUNCTION: Math.mulDiv(uint256,uint256,uint256) (NodeID: 68)
    │       │     💬 Args: [aToken.scaledBalanceOf(address(this)), liquidityIndex, WadRayMath.RAY]
    │       │     👁️  Def: internal
    │       │   ├─ [8] ⚙️ FUNCTION: Math.mul512(uint256,uint256) (NodeID: 69)
    │       │   │   💬 Args: [x, y]
    │       │   │   👁️  Def: internal
    │       │   └─ [8] ⚙️ FUNCTION: Panic.panic(uint256) (NodeID: 70)
    │       │       💬 Args: [ternary(denominator == 0, Panic.DIVISION_BY_ZERO, Panic.UNDER_OVERFLOW)]
    │       │       👁️  Def: internal
    │       │     └─ [9] ⚙️ FUNCTION: Math.ternary(bool,uint256,uint256) (NodeID: 71)
    │       │         💬 Args: [denominator == 0, Panic.DIVISION_BY_ZERO, Panic.UNDER_OVERFLOW]
    │       │         👁️  Def: internal
    │       │       └─ [10] ⚙️ FUNCTION: SafeCast.toUint(bool) (NodeID: 72)
    │       │           💬 Args: [condition]
    │       │           👁️  Def: internal
    │       ├─ [6] ⚙️ FUNCTION: SafeCast.toUint(bool) (NodeID: 55)
    │       │   💬 Args: [unsignedRoundsUp(rounding) && (mulmod(x, y, denominator) > 0)]
    │       │   👁️  Def: internal
    │       │ └─ [7] ⚙️ FUNCTION: Math.unsignedRoundsUp(enum Math.Rounding) (NodeID: 56)
    │       │     💬 Args: [rounding]
    │       │     👁️  Def: internal
    │       └─ [6] ⚙️ FUNCTION: Math.mulDiv(uint256,uint256,uint256) (NodeID: 57)
    │           💬 Args: [x, y, denominator]
    │           👁️  Def: internal
    │         ├─ [7] ⚙️ FUNCTION: Math.mul512(uint256,uint256) (NodeID: 58)
    │         │   💬 Args: [x, y]
    │         │   👁️  Def: internal
    │         └─ [7] ⚙️ FUNCTION: Panic.panic(uint256) (NodeID: 59)
    │             💬 Args: [ternary(denominator == 0, Panic.DIVISION_BY_ZERO, Panic.UNDER_OVERFLOW)]
    │             👁️  Def: internal
    │           └─ [8] ⚙️ FUNCTION: Math.ternary(bool,uint256,uint256) (NodeID: 60)
    │               💬 Args: [denominator == 0, Panic.DIVISION_BY_ZERO, Panic.UNDER_OVERFLOW]
    │               👁️  Def: internal
    │             └─ [9] ⚙️ FUNCTION: SafeCast.toUint(bool) (NodeID: 61)
    │                 💬 Args: [condition]
    │                 👁️  Def: internal
    └─ [2] ⚙️ FUNCTION: Math.ternary(bool,uint256,uint256) (NodeID: 2)
        💬 Args: [a < b, a, b]
        👁️  Def: internal
      └─ [3] ⚙️ FUNCTION: SafeCast.toUint(bool) (NodeID: 3)
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
