# Function: rebalance(contract IVault,contract IVault,uint256,uint256)

**Contract**: [src/VeryLiquidVault.sol/contract_VeryLiquidVault.md]

## Metadata

- **Contract**: VeryLiquidVault
- **Signature**: `rebalance(contract IVault,contract IVault,uint256,uint256)`
- **Visibility**: external
- **Source Range**: 16239:772:145

## Implementation

```solidity
/// @notice Rebalances assets between two strategies
///  @param strategyFrom The strategy to transfer assets from
///  @param strategyTo The strategy to transfer assets to
///  @param amount The amount of assets to transfer
///  @param maxSlippagePercent The maximum slippage percent allowed for the rebalance
///  @dev Only callable by addresses with STRATEGIST_ROLE
///  @dev Transfers assets from one strategy to another
///  @dev We have maxSlippagePercent <= PERCENT since rebalanceMaxSlippagePercent has already been checked in setRebalanceMaxSlippagePercent
function rebalance(IVault strategyFrom, IVault strategyTo, uint256 amount, uint256 maxSlippagePercent) external nonReentrant() notPaused() emitVaultStatus() onlyAuth(STRATEGIST_ROLE) {
    maxSlippagePercent = Math.min(maxSlippagePercent, _rebalanceMaxSlippagePercent());
    amount = Math.min(amount, strategyFrom.maxWithdraw(address(this)));
    if (!_isStrategy(strategyFrom)) revert InvalidStrategy(address(strategyFrom));
    if (!_isStrategy(strategyTo)) revert InvalidStrategy(address(strategyTo));
    if (strategyFrom == strategyTo) revert InvalidStrategy(address(strategyTo));
    if (amount == 0) revert NullAmount();
    _rebalance(strategyFrom, strategyTo, amount, maxSlippagePercent);
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

### _rebalanceMaxSlippagePercent()

- **Kind**: internal
- **Source**: 25233:152:145
- **Link**: `src/VeryLiquidVault.sol:VeryLiquidVault:_rebalanceMaxSlippagePercent()`

```solidity
/// @notice Internal function to get the rebalance max slippage percent
function _rebalanceMaxSlippagePercent() private view returns (uint256) {
    return _getVeryLiquidVaultStorage()._rebalanceMaxSlippagePercent;
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

### _isStrategy(contract IVault)

- **Kind**: internal
- **Source**: 25758:331:145
- **Link**: `src/VeryLiquidVault.sol:VeryLiquidVault:_isStrategy(contract IVault)`

```solidity
/// @notice Internal function to check if the strategy is in the vault
function _isStrategy(IVault strategy) private view returns (bool) {
    VeryLiquidVaultStorage storage $ = _getVeryLiquidVaultStorage();
    uint256 length = $._strategies.length;
    for (uint256 i = 0; i < length; ++i) {
        if ($._strategies[i] == strategy) return true;
    }
    return false;
}
```

### _rebalance(contract IVault,contract IVault,uint256,uint256)

- **Kind**: internal
- **Source**: 22495:1413:145
- **Link**: `src/VeryLiquidVault.sol:VeryLiquidVault:_rebalance(contract IVault,contract IVault,uint256,uint256)`

```solidity
/// @notice Internal function to rebalance assets between two strategies
///  @dev If before - after > maxSlippagePercent * amount, the _rebalance operation reverts
///  @dev Assumes input is validated by caller functions
///  @param strategyFrom The strategy to transfer assets from
///  @param strategyTo The strategy to transfer assets to
///  @param amount The amount of assets to transfer
///  @param maxSlippagePercent The maximum slippage percent allowed for the rebalance
function _rebalance(IVault strategyFrom, IVault strategyTo, uint256 amount, uint256 maxSlippagePercent) private {
    uint256 assetsBefore = strategyFrom.convertToAssets(strategyFrom.balanceOf(address(this))) + strategyTo.convertToAssets(strategyTo.balanceOf(address(this)));
    uint256 balanceBefore = IERC20(asset()).balanceOf(address(this));
    if (amount > 0) {
        strategyFrom.withdraw(amount, address(this), address(this));
    }
    uint256 balanceAfter = IERC20(asset()).balanceOf(address(this));
    uint256 assets = balanceAfter - balanceBefore;
    if (assets > 0) {
        IERC20(asset()).forceApprove(address(strategyTo), assets);
        strategyTo.deposit(assets, address(this));
    }
    uint256 assetsAfter = strategyFrom.convertToAssets(strategyFrom.balanceOf(address(this))) + strategyTo.convertToAssets(strategyTo.balanceOf(address(this)));
    uint256 slippage = Math.mulDiv(maxSlippagePercent, amount, PERCENT);
    if (assetsBefore > (slippage + assetsAfter)) {
        revert TransferredAmountLessThanMin(assetsBefore, assetsAfter, slippage, amount, maxSlippagePercent);
    }
    emit Rebalanced(address(strategyFrom), address(strategyTo), assets, maxSlippagePercent);
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

### onlyAuth(bytes32)

- **Kind**: modifier
- **Source**: 4783:80:149
- **Link**: `src/utils/BaseVault.sol:BaseVault:onlyAuth(bytes32)`

```solidity
/// @notice Modifier to restrict function access to addresses with specific roles
///  @dev Reverts if the caller doesn't have the required role
modifier onlyAuth(bytes32 role) {
    _checkAuthRole(role);
    _;
}
```

### _checkAuthRole(bytes32)

- **Kind**: internal
- **Source**: 5663:208:149
- **Link**: `src/utils/BaseVault.sol:BaseVault:_checkAuthRole(bytes32)`

```solidity
/// @notice Checks that the caller has the required role
///  @dev Reverts if the caller doesn't have the required role
function _checkAuthRole(bytes32 role) internal view {
    if (!auth().hasRole(role, _msgSender())) {
        revert IAccessControl.AccessControlUnauthorizedAccount(_msgSender(), role);
    }
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

### _msgSender()

- **Kind**: internal
- **Source**: 887:96:61
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/utils/ContextUpgradeable.sol:ContextUpgradeable:_msgSender()`

```solidity
function _msgSender() virtual internal view returns (address) {
    return msg.sender;
}
```

### emitVaultStatus()

- **Kind**: modifier
- **Source**: 5211:73:149
- **Link**: `src/utils/BaseVault.sol:BaseVault:emitVaultStatus()`

```solidity
/// @notice Modifier to emit the vault status
///  @dev Emits the vault status after the function is executed
modifier emitVaultStatus() {
    _;
    _emitVaultStatus();
}
```

### _emitVaultStatus()

- **Kind**: internal
- **Source**: 5983:100:149
- **Link**: `src/utils/BaseVault.sol:BaseVault:_emitVaultStatus()`

```solidity
/// @notice Emits the vault status
///  @dev Emits the vault status after the function is executed
function _emitVaultStatus() internal {
    emit VaultStatus(totalSupply(), totalAssets());
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

### notPaused()

- **Kind**: modifier
- **Source**: 5007:81:149
- **Link**: `src/utils/BaseVault.sol:BaseVault:notPaused()`

```solidity
/// @notice Modifier to ensure the contract is not paused
///  @dev Checks both local pause state and global pause state from Auth
modifier notPaused() {
    _requireNotPausedAuthNotPaused();
    _;
}
```

### _requireNotPausedAuthNotPaused()

- **Kind**: internal
- **Source**: 6207:122:149
- **Link**: `src/utils/BaseVault.sol:BaseVault:_requireNotPausedAuthNotPaused()`

```solidity
/// @notice Internal function to require that the vault is not paused
///  @dev Reverts if the vault is paused
function _requireNotPausedAuthNotPaused() internal view {
    if (_pausedOrAuthPaused()) revert EnforcedPause();
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

### nonReentrant()

- **Kind**: modifier
- **Source**: 3361:103:65
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/utils/ReentrancyGuardUpgradeable.sol:ReentrancyGuardUpgradeable:nonReentrant()`

```solidity
///  @dev Prevents a contract from calling itself, directly or indirectly.
///  Calling a `nonReentrant` function from another `nonReentrant`
///  function is not supported. It is possible to prevent this from happening
///  by making the `nonReentrant` function external, and making it call a
///  `private` function that does the actual work.
modifier nonReentrant() {
    _nonReentrantBefore();
    _;
    _nonReentrantAfter();
}
```

### _nonReentrantBefore()

- **Kind**: internal
- **Source**: 3470:384:65
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/utils/ReentrancyGuardUpgradeable.sol:ReentrancyGuardUpgradeable:_nonReentrantBefore()`

```solidity
function _nonReentrantBefore() private {
    ReentrancyGuardStorage storage $ = _getReentrancyGuardStorage();
    if ($._status == ENTERED) {
        revert ReentrancyGuardReentrantCall();
    }
    $._status = ENTERED;
}
```

### _getReentrancyGuardStorage()

- **Kind**: internal
- **Source**: 2395:183:65
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/utils/ReentrancyGuardUpgradeable.sol:ReentrancyGuardUpgradeable:_getReentrancyGuardStorage()`

```solidity
function _getReentrancyGuardStorage() private pure returns (ReentrancyGuardStorage storage $) {
    assembly {
        $.slot := ReentrancyGuardStorageLocation
    }
}
```

### _nonReentrantAfter()

- **Kind**: internal
- **Source**: 3860:283:65
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/utils/ReentrancyGuardUpgradeable.sol:ReentrancyGuardUpgradeable:_nonReentrantAfter()`

```solidity
function _nonReentrantAfter() private {
    ReentrancyGuardStorage storage $ = _getReentrancyGuardStorage();
    $._status = NOT_ENTERED;
}
```

## External Calls

- **IVault::maxWithdraw(address)**

## State Variable Reads

- **ENTERED** (`uint256`)
- **NOT_ENTERED** (`uint256`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: VeryLiquidVault.rebalance(contract IVault,contract IVault,uint256,uint256) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: external
  ├─ [1] ⚙️ FUNCTION: Math.min(uint256,uint256) (NodeID: 1)
  │   💬 Args: [maxSlippagePercent, _rebalanceMaxSlippagePercent()]
  │   👁️  Def: internal
  │ ├─ [2] ⚙️ FUNCTION: VeryLiquidVault._rebalanceMaxSlippagePercent() (NodeID: 4)
  │ │   💬 Args: [no args]
  │ │   👁️  Def: private
  │ │ └─ [3] ⚙️ FUNCTION: VeryLiquidVault._getVeryLiquidVaultStorage() (NodeID: 5)
  │ │     💬 Args: [no args]
  │ │     👁️  Def: private
  │ └─ [2] ⚙️ FUNCTION: Math.ternary(bool,uint256,uint256) (NodeID: 2)
  │     💬 Args: [a < b, a, b]
  │     👁️  Def: internal
  │   └─ [3] ⚙️ FUNCTION: SafeCast.toUint(bool) (NodeID: 3)
  │       💬 Args: [condition]
  │       👁️  Def: internal
  ├─ [1] ⚙️ FUNCTION: Math.min(uint256,uint256) (NodeID: 6)
  │   💬 Args: [amount, strategyFrom.maxWithdraw(address(this))]
  │   👁️  Def: internal
  │ └─ [2] ⚙️ FUNCTION: Math.ternary(bool,uint256,uint256) (NodeID: 7)
  │     💬 Args: [a < b, a, b]
  │     👁️  Def: internal
  │   └─ [3] ⚙️ FUNCTION: SafeCast.toUint(bool) (NodeID: 8)
  │       💬 Args: [condition]
  │       👁️  Def: internal
  ├─ [1] ⚙️ FUNCTION: VeryLiquidVault._isStrategy(contract IVault) (NodeID: 9)
  │   💬 Args: [strategyFrom]
  │   👁️  Def: private
  │ └─ [2] ⚙️ FUNCTION: VeryLiquidVault._getVeryLiquidVaultStorage() (NodeID: 10)
  │     💬 Args: [no args]
  │     👁️  Def: private
  ├─ [1] ⚙️ FUNCTION: VeryLiquidVault._isStrategy(contract IVault) (NodeID: 11)
  │   💬 Args: [strategyTo]
  │   👁️  Def: private
  │ └─ [2] ⚙️ FUNCTION: VeryLiquidVault._getVeryLiquidVaultStorage() (NodeID: 12)
  │     💬 Args: [no args]
  │     👁️  Def: private
  ├─ [1] ⚙️ FUNCTION: VeryLiquidVault._rebalance(contract IVault,contract IVault,uint256,uint256) (NodeID: 13)
  │   💬 Args: [strategyFrom, strategyTo, amount, maxSlippagePercent]
  │   👁️  Def: private
  │ ├─ [2] ⚙️ FUNCTION: ERC4626Upgradeable.asset() (NodeID: 14)
  │ │   💬 Args: [no args]
  │ │   👁️  Def: public
  │ │ └─ [3] ⚙️ FUNCTION: ERC4626Upgradeable._getERC4626Storage() (NodeID: 15)
  │ │     💬 Args: [no args]
  │ │     👁️  Def: private
  │ ├─ [2] ⚙️ FUNCTION: ERC4626Upgradeable.asset() (NodeID: 16)
  │ │   💬 Args: [no args]
  │ │   👁️  Def: public
  │ │ └─ [3] ⚙️ FUNCTION: ERC4626Upgradeable._getERC4626Storage() (NodeID: 17)
  │ │     💬 Args: [no args]
  │ │     👁️  Def: private
  │ ├─ [2] ⚙️ FUNCTION: ERC4626Upgradeable.asset() (NodeID: 18)
  │ │   💬 Args: [no args]
  │ │   👁️  Def: public
  │ │ └─ [3] ⚙️ FUNCTION: ERC4626Upgradeable._getERC4626Storage() (NodeID: 19)
  │ │     💬 Args: [no args]
  │ │     👁️  Def: private
  │ └─ [2] ⚙️ FUNCTION: Math.mulDiv(uint256,uint256,uint256) (NodeID: 20)
  │     💬 Args: [maxSlippagePercent, amount, PERCENT]
  │     👁️  Def: internal
  │   ├─ [3] ⚙️ FUNCTION: Math.mul512(uint256,uint256) (NodeID: 21)
  │   │   💬 Args: [x, y]
  │   │   👁️  Def: internal
  │   └─ [3] ⚙️ FUNCTION: Panic.panic(uint256) (NodeID: 22)
  │       💬 Args: [ternary(denominator == 0, Panic.DIVISION_BY_ZERO, Panic.UNDER_OVERFLOW)]
  │       👁️  Def: internal
  │     └─ [4] ⚙️ FUNCTION: Math.ternary(bool,uint256,uint256) (NodeID: 23)
  │         💬 Args: [denominator == 0, Panic.DIVISION_BY_ZERO, Panic.UNDER_OVERFLOW]
  │         👁️  Def: internal
  │       └─ [5] ⚙️ FUNCTION: SafeCast.toUint(bool) (NodeID: 24)
  │           💬 Args: [condition]
  │           👁️  Def: internal
  ├─ [1] 🔒 MODIFIER: BaseVault.onlyAuth(bytes32) (NodeID: 25)
  │   💬 Args: [STRATEGIST_ROLE]
  │ └─ [2] ⚙️ FUNCTION: BaseVault._checkAuthRole(bytes32) (NodeID: 26)
  │     💬 Args: [STRATEGIST_ROLE]
  │     👁️  Def: internal
  │   ├─ [3] ⚙️ FUNCTION: BaseVault.auth() (NodeID: 27)
  │   │   💬 Args: [no args]
  │   │   👁️  Def: public
  │   │ └─ [4] ⚙️ FUNCTION: BaseVault._getBaseVaultStorage() (NodeID: 28)
  │   │     💬 Args: [no args]
  │   │     👁️  Def: private
  │   ├─ [3] ⚙️ FUNCTION: ContextUpgradeable._msgSender() (NodeID: 29)
  │   │   💬 Args: [no args]
  │   │   👁️  Def: internal
  │   └─ [3] ⚙️ FUNCTION: ContextUpgradeable._msgSender() (NodeID: 30)
  │       💬 Args: [no args]
  │       👁️  Def: internal
  ├─ [1] 🔒 MODIFIER: BaseVault.emitVaultStatus() (NodeID: 31)
  │   💬 Args: [no args]
  │ └─ [2] ⚙️ FUNCTION: BaseVault._emitVaultStatus() (NodeID: 32)
  │     💬 Args: [no args]
  │     👁️  Def: internal
  │   ├─ [3] ⚙️ FUNCTION: ERC20Upgradeable.totalSupply() (NodeID: 33)
  │   │   💬 Args: [no args]
  │   │   👁️  Def: public
  │   │ └─ [4] ⚙️ FUNCTION: ERC20Upgradeable._getERC20Storage() (NodeID: 34)
  │   │     💬 Args: [no args]
  │   │     👁️  Def: private
  │   └─ [3] ⚙️ FUNCTION: VeryLiquidVault.totalAssets() (NodeID: 35)
  │       💬 Args: [no args]
  │       👁️  Def: public
  │     └─ [4] ⚙️ FUNCTION: VeryLiquidVault._getVeryLiquidVaultStorage() (NodeID: 36)
  │         💬 Args: [no args]
  │         👁️  Def: private
  ├─ [1] 🔒 MODIFIER: BaseVault.notPaused() (NodeID: 37)
  │   💬 Args: [no args]
  │ └─ [2] ⚙️ FUNCTION: BaseVault._requireNotPausedAuthNotPaused() (NodeID: 38)
  │     💬 Args: [no args]
  │     👁️  Def: internal
  │   └─ [3] ⚙️ FUNCTION: BaseVault._pausedOrAuthPaused() (NodeID: 39)
  │       💬 Args: [no args]
  │       👁️  Def: private
  │     ├─ [4] ⚙️ FUNCTION: BaseVault.auth() (NodeID: 40)
  │     │   💬 Args: [no args]
  │     │   👁️  Def: public
  │     │ └─ [5] ⚙️ FUNCTION: BaseVault._getBaseVaultStorage() (NodeID: 41)
  │     │     💬 Args: [no args]
  │     │     👁️  Def: private
  │     └─ [4] ⚙️ FUNCTION: PausableUpgradeable.paused() (NodeID: 42)
  │         💬 Args: [no args]
  │         👁️  Def: public
  │       └─ [5] ⚙️ FUNCTION: PausableUpgradeable._getPausableStorage() (NodeID: 43)
  │           💬 Args: [no args]
  │           👁️  Def: private
  └─ [1] 🔒 MODIFIER: ReentrancyGuardUpgradeable.nonReentrant() (NodeID: 44)
      💬 Args: [no args]
    ├─ [2] ⚙️ FUNCTION: ReentrancyGuardUpgradeable._nonReentrantBefore() (NodeID: 45)
    │   💬 Args: [no args]
    │   👁️  Def: private
    │ └─ [3] ⚙️ FUNCTION: ReentrancyGuardUpgradeable._getReentrancyGuardStorage() (NodeID: 46)
    │     💬 Args: [no args]
    │     👁️  Def: private
    └─ [2] ⚙️ FUNCTION: ReentrancyGuardUpgradeable._nonReentrantAfter() (NodeID: 47)
        💬 Args: [no args]
        👁️  Def: private
      └─ [3] ⚙️ FUNCTION: ReentrancyGuardUpgradeable._getReentrancyGuardStorage() (NodeID: 48)
          💬 Args: [no args]
          👁️  Def: private
```

## Documentation

### Function Documentation

@notice Rebalances assets between two strategies
 @param strategyFrom The strategy to transfer assets from
 @param strategyTo The strategy to transfer assets to
 @param amount The amount of assets to transfer
 @param maxSlippagePercent The maximum slippage percent allowed for the rebalance
 @dev Only callable by addresses with STRATEGIST_ROLE
 @dev Transfers assets from one strategy to another
 @dev We have maxSlippagePercent <= PERCENT since rebalanceMaxSlippagePercent has already been checked in setRebalanceMaxSlippagePercent
