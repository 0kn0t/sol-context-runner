# Function: addStrategy(contract IVault)

**Contract**: [src/VeryLiquidVault.sol/contract_VeryLiquidVault.md]

## Metadata

- **Contract**: VeryLiquidVault
- **Signature**: `addStrategy(contract IVault)`
- **Visibility**: external
- **Source Range**: 12033:172:145

## Implementation

```solidity
/// @notice Adds a new strategy to the vault
///  @param strategy_ The new strategy to add
///  @dev Only callable by addresses with VAULT_MANAGER_ROLE
function addStrategy(IVault strategy_) external nonReentrant() emitVaultStatus() onlyAuth(VAULT_MANAGER_ROLE) {
    _addStrategy(strategy_, asset(), address(auth()));
}
```

## Related Implementations

### _addStrategy(contract IVault,address,address)

- **Kind**: internal
- **Source**: 17386:661:145
- **Link**: `src/VeryLiquidVault.sol:VeryLiquidVault:_addStrategy(contract IVault,address,address)`

```solidity
/// @notice Internal function to add a strategy
///  @param strategy_ The strategy to add
///  @param asset_ The asset of the strategy
///  @param auth_ The auth of the strategy
///  @dev Strategy configuration is assumed to be correct (non-malicious, no circular dependencies, etc.)
function _addStrategy(IVault strategy_, address asset_, address auth_) private {
    if (address(strategy_) == address(0)) revert NullAddress();
    if (_isStrategy(strategy_)) revert InvalidStrategy(address(strategy_));
    if ((strategy_.asset() != asset_) || (address(strategy_.auth()) != auth_)) {
        revert InvalidStrategy(address(strategy_));
    }
    VeryLiquidVaultStorage storage $ = _getVeryLiquidVaultStorage();
    $._strategies.push(strategy_);
    emit StrategyAdded(address(strategy_));
    if ($._strategies.length > MAX_STRATEGIES) revert MaxStrategiesExceeded($._strategies.length, MAX_STRATEGIES);
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

## State Variable Reads

- **MAX_STRATEGIES** (`uint256`)
- **ENTERED** (`uint256`)
- **NOT_ENTERED** (`uint256`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: VeryLiquidVault.addStrategy(contract IVault) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: external
  ├─ [1] ⚙️ FUNCTION: VeryLiquidVault._addStrategy(contract IVault,address,address) (NodeID: 1)
  │   💬 Args: [strategy_, asset(), address(auth())]
  │   👁️  Def: private
  │ ├─ [2] ⚙️ FUNCTION: ERC4626Upgradeable.asset() (NodeID: 5)
  │ │   💬 Args: [no args]
  │ │   👁️  Def: public
  │ │ └─ [3] ⚙️ FUNCTION: ERC4626Upgradeable._getERC4626Storage() (NodeID: 6)
  │ │     💬 Args: [no args]
  │ │     👁️  Def: private
  │ ├─ [2] ⚙️ FUNCTION: BaseVault.auth() (NodeID: 7)
  │ │   💬 Args: [no args]
  │ │   👁️  Def: public
  │ │ └─ [3] ⚙️ FUNCTION: BaseVault._getBaseVaultStorage() (NodeID: 8)
  │ │     💬 Args: [no args]
  │ │     👁️  Def: private
  │ ├─ [2] ⚙️ FUNCTION: VeryLiquidVault._isStrategy(contract IVault) (NodeID: 2)
  │ │   💬 Args: [strategy_]
  │ │   👁️  Def: private
  │ │ └─ [3] ⚙️ FUNCTION: VeryLiquidVault._getVeryLiquidVaultStorage() (NodeID: 3)
  │ │     💬 Args: [no args]
  │ │     👁️  Def: private
  │ └─ [2] ⚙️ FUNCTION: VeryLiquidVault._getVeryLiquidVaultStorage() (NodeID: 4)
  │     💬 Args: [no args]
  │     👁️  Def: private
  ├─ [1] 🔒 MODIFIER: BaseVault.onlyAuth(bytes32) (NodeID: 9)
  │   💬 Args: [VAULT_MANAGER_ROLE]
  │ └─ [2] ⚙️ FUNCTION: BaseVault._checkAuthRole(bytes32) (NodeID: 10)
  │     💬 Args: [VAULT_MANAGER_ROLE]
  │     👁️  Def: internal
  │   ├─ [3] ⚙️ FUNCTION: BaseVault.auth() (NodeID: 11)
  │   │   💬 Args: [no args]
  │   │   👁️  Def: public
  │   │ └─ [4] ⚙️ FUNCTION: BaseVault._getBaseVaultStorage() (NodeID: 12)
  │   │     💬 Args: [no args]
  │   │     👁️  Def: private
  │   ├─ [3] ⚙️ FUNCTION: ContextUpgradeable._msgSender() (NodeID: 13)
  │   │   💬 Args: [no args]
  │   │   👁️  Def: internal
  │   └─ [3] ⚙️ FUNCTION: ContextUpgradeable._msgSender() (NodeID: 14)
  │       💬 Args: [no args]
  │       👁️  Def: internal
  ├─ [1] 🔒 MODIFIER: BaseVault.emitVaultStatus() (NodeID: 15)
  │   💬 Args: [no args]
  │ └─ [2] ⚙️ FUNCTION: BaseVault._emitVaultStatus() (NodeID: 16)
  │     💬 Args: [no args]
  │     👁️  Def: internal
  │   ├─ [3] ⚙️ FUNCTION: ERC20Upgradeable.totalSupply() (NodeID: 17)
  │   │   💬 Args: [no args]
  │   │   👁️  Def: public
  │   │ └─ [4] ⚙️ FUNCTION: ERC20Upgradeable._getERC20Storage() (NodeID: 18)
  │   │     💬 Args: [no args]
  │   │     👁️  Def: private
  │   └─ [3] ⚙️ FUNCTION: VeryLiquidVault.totalAssets() (NodeID: 19)
  │       💬 Args: [no args]
  │       👁️  Def: public
  │     └─ [4] ⚙️ FUNCTION: VeryLiquidVault._getVeryLiquidVaultStorage() (NodeID: 20)
  │         💬 Args: [no args]
  │         👁️  Def: private
  └─ [1] 🔒 MODIFIER: ReentrancyGuardUpgradeable.nonReentrant() (NodeID: 21)
      💬 Args: [no args]
    ├─ [2] ⚙️ FUNCTION: ReentrancyGuardUpgradeable._nonReentrantBefore() (NodeID: 22)
    │   💬 Args: [no args]
    │   👁️  Def: private
    │ └─ [3] ⚙️ FUNCTION: ReentrancyGuardUpgradeable._getReentrancyGuardStorage() (NodeID: 23)
    │     💬 Args: [no args]
    │     👁️  Def: private
    └─ [2] ⚙️ FUNCTION: ReentrancyGuardUpgradeable._nonReentrantAfter() (NodeID: 24)
        💬 Args: [no args]
        👁️  Def: private
      └─ [3] ⚙️ FUNCTION: ReentrancyGuardUpgradeable._getReentrancyGuardStorage() (NodeID: 25)
          💬 Args: [no args]
          👁️  Def: private
```

## Documentation

### Function Documentation

@notice Adds a new strategy to the vault
 @param strategy_ The new strategy to add
 @dev Only callable by addresses with VAULT_MANAGER_ROLE
