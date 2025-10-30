# Function: maxDeposit(address)

**Contract**: [src/VeryLiquidVault.sol/contract_VeryLiquidVault.md]

## Metadata

- **Contract**: VeryLiquidVault
- **Signature**: `maxDeposit(address)`
- **Visibility**: public
- **Source Range**: 4944:175:145

## Implementation

```solidity
/// @inheritdoc ERC4626Upgradeable
///  @dev The maximum amount that can be deposited is the minimum between this receiver specific limit and the maximum asset amount that can be deposited to all strategies
function maxDeposit(address receiver) override(BaseVault) public view returns (uint256) {
    return Math.min(_maxDepositToStrategies(), super.maxDeposit(receiver));
}
```

## Related Implementations

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

### _maxDepositToStrategies()

- **Kind**: internal
- **Source**: 20365:414:145
- **Link**: `src/VeryLiquidVault.sol:VeryLiquidVault:_maxDepositToStrategies()`

```solidity
/// @notice Internal function to calculate maximum depositable amount in all strategies
///  @dev The maximum amount that can be deposited to all strategies is the sum of the maximum amount that can be deposited to each strategy
///  @dev This value might be overstated if nested strategies are used. For example, if a very liquid has two strategies, one of which is an ERC4626StrategyVault and the other is a VeryLiquidVault that has the same ERC4626StrategyVault instance. In this scenario, if the ERC-4626 strategy has 100 maxDeposit remaining, the top-level very liquid would double count this value and return 200. However, in practice, trying to deposit 200 would cause a revert, because only 100 can be deposited.
function _maxDepositToStrategies() private view returns (uint256 maxAssets) {
    VeryLiquidVaultStorage storage $ = _getVeryLiquidVaultStorage();
    uint256 length = $._strategies.length;
    for (uint256 i = 0; i < length; ++i) {
        maxAssets = Math.saturatingAdd(maxAssets, $._strategies[i].maxDeposit(address(this)));
        if (maxAssets == type(uint256).max) break;
    }
}
```

### _getVeryLiquidVaultStorage()

- **Kind**: internal
- **Source**: 1874:183:145
- **Link**: `src/VeryLiquidVault.sol:VeryLiquidVault:_getVeryLiquidVaultStorage()`

```solidity
function _getVeryLiquidVaultStorage() private pure returns (VeryLiquidVaultStorage storage $) {
    assembly {
        $.slot := VeryLiquidVaultStorageLocation
    }
}
```

### saturatingAdd(uint256,uint256)

- **Kind**: internal
- **Source**: 3912:199:109
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
- **Source**: 1693:240:109
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
- **Source**: 7057:583:145
- **Link**: `src/VeryLiquidVault.sol:VeryLiquidVault:totalAssets()`

```solidity
/// @inheritdoc ERC4626Upgradeable
///  @dev The total assets is the sum of the assets in all strategies
function totalAssets() virtual override(ERC4626Upgradeable, IERC4626) public view returns (uint256 total) {
    VeryLiquidVaultStorage storage $ = _getVeryLiquidVaultStorage();
    uint256 length = $._strategies.length;
    for (uint256 i = 0; i < length; ++i) {
        IVault strategy = $._strategies[i];
        uint256 strategyBalance = strategy.balanceOf(address(this));
        if (strategyBalance == 0) continue;
        total += strategy.convertToAssets(strategyBalance);
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

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: VeryLiquidVault.maxDeposit(address) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
  └─ [1] ⚙️ FUNCTION: Math.min(uint256,uint256) (NodeID: 1)
      💬 Args: [_maxDepositToStrategies(), super.maxDeposit(receiver)]
      👁️  Def: internal
    ├─ [2] ⚙️ FUNCTION: VeryLiquidVault._maxDepositToStrategies() (NodeID: 4)
    │   💬 Args: [no args]
    │   👁️  Def: private
    │ ├─ [3] ⚙️ FUNCTION: VeryLiquidVault._getVeryLiquidVaultStorage() (NodeID: 5)
    │ │   💬 Args: [no args]
    │ │   👁️  Def: private
    │ └─ [3] ⚙️ FUNCTION: Math.saturatingAdd(uint256,uint256) (NodeID: 6)
    │     💬 Args: [maxAssets, $._strategies[i].maxDeposit(address(this))]
    │     👁️  Def: internal
    │   ├─ [4] ⚙️ FUNCTION: Math.tryAdd(uint256,uint256) (NodeID: 7)
    │   │   💬 Args: [a, b]
    │   │   👁️  Def: internal
    │   │ └─ [5] ⚙️ FUNCTION: SafeCast.toUint(bool) (NodeID: 8)
    │   │     💬 Args: [success]
    │   │     👁️  Def: internal
    │   └─ [4] ⚙️ FUNCTION: Math.ternary(bool,uint256,uint256) (NodeID: 9)
    │       💬 Args: [success, result, type(uint256).max]
    │       👁️  Def: internal
    │     └─ [5] ⚙️ FUNCTION: SafeCast.toUint(bool) (NodeID: 10)
    │         💬 Args: [condition]
    │         👁️  Def: internal
    ├─ [2] ⚙️ FUNCTION: BaseVault.maxDeposit(address) (NodeID: 11)
    │   💬 Args: [receiver]
    │   👁️  Def: public
    │ ├─ [3] ⚙️ FUNCTION: BaseVault._pausedOrAuthPaused() (NodeID: 12)
    │ │   💬 Args: [no args]
    │ │   👁️  Def: private
    │ │ ├─ [4] ⚙️ FUNCTION: BaseVault.auth() (NodeID: 13)
    │ │ │   💬 Args: [no args]
    │ │ │   👁️  Def: public
    │ │ │ └─ [5] ⚙️ FUNCTION: BaseVault._getBaseVaultStorage() (NodeID: 14)
    │ │ │     💬 Args: [no args]
    │ │ │     👁️  Def: private
    │ │ └─ [4] ⚙️ FUNCTION: PausableUpgradeable.paused() (NodeID: 15)
    │ │     💬 Args: [no args]
    │ │     👁️  Def: public
    │ │   └─ [5] ⚙️ FUNCTION: PausableUpgradeable._getPausableStorage() (NodeID: 16)
    │ │       💬 Args: [no args]
    │ │       👁️  Def: private
    │ ├─ [3] ⚙️ FUNCTION: BaseVault._totalAssetsCap() (NodeID: 17)
    │ │   💬 Args: [no args]
    │ │   👁️  Def: private
    │ │ └─ [4] ⚙️ FUNCTION: BaseVault._getBaseVaultStorage() (NodeID: 18)
    │ │     💬 Args: [no args]
    │ │     👁️  Def: private
    │ ├─ [3] ⚙️ FUNCTION: ERC4626Upgradeable.maxDeposit(address) (NodeID: 19)
    │ │   💬 Args: [receiver]
    │ │   👁️  Def: public
    │ └─ [3] ⚙️ FUNCTION: BaseVault._maxDeposit() (NodeID: 20)
    │     💬 Args: [no args]
    │     👁️  Def: private
    │   └─ [4] ⚙️ FUNCTION: Math.saturatingSub(uint256,uint256) (NodeID: 21)
    │       💬 Args: [_totalAssetsCap(), totalAssets()]
    │       👁️  Def: internal
    │     ├─ [5] ⚙️ FUNCTION: BaseVault._totalAssetsCap() (NodeID: 24)
    │     │   💬 Args: [no args]
    │     │   👁️  Def: private
    │     │ └─ [6] ⚙️ FUNCTION: BaseVault._getBaseVaultStorage() (NodeID: 25)
    │     │     💬 Args: [no args]
    │     │     👁️  Def: private
    │     ├─ [5] ⚙️ FUNCTION: VeryLiquidVault.totalAssets() (NodeID: 26)
    │     │   💬 Args: [no args]
    │     │   👁️  Def: public
    │     │ └─ [6] ⚙️ FUNCTION: VeryLiquidVault._getVeryLiquidVaultStorage() (NodeID: 27)
    │     │     💬 Args: [no args]
    │     │     👁️  Def: private
    │     └─ [5] ⚙️ FUNCTION: Math.trySub(uint256,uint256) (NodeID: 22)
    │         💬 Args: [a, b]
    │         👁️  Def: internal
    │       └─ [6] ⚙️ FUNCTION: SafeCast.toUint(bool) (NodeID: 23)
    │           💬 Args: [success]
    │           👁️  Def: internal
    └─ [2] ⚙️ FUNCTION: Math.ternary(bool,uint256,uint256) (NodeID: 2)
        💬 Args: [a < b, a, b]
        👁️  Def: internal
      └─ [3] ⚙️ FUNCTION: SafeCast.toUint(bool) (NodeID: 3)
          💬 Args: [condition]
          👁️  Def: internal
```

## Documentation

### Function Documentation

@inheritdoc ERC4626Upgradeable
 @dev The maximum amount that can be deposited is the minimum between this receiver specific limit and the maximum asset amount that can be deposited to all strategies

### Interface Documentation

 @dev Returns the maximum amount of the underlying asset that can be deposited into the Vault for the receiver,
 through a deposit call.
 - MUST return a limited value if receiver is subject to some deposit limit.
 - MUST return 2 ** 256 - 1 if there is no limit on the maximum amount of assets that may be deposited.
 - MUST NOT revert.
