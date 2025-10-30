# Function: reorderStrategies(contract IVault[])

**Contract**: [src/VeryLiquidVault.sol/contract_VeryLiquidVault.md]

## Metadata

- **Contract**: VeryLiquidVault
- **Signature**: `reorderStrategies(contract IVault[])`
- **Visibility**: external
- **Source Range**: 14588:1054:145

## Implementation

```solidity
/// @notice Reorders the strategies
///  @param newStrategiesOrder The new strategies order
///  @dev Only callable by addresses with STRATEGIST_ROLE
///  @dev Verifies that the new strategies order is valid and that there are no duplicates
///  @dev Clears current strategies and adds them in the new order
function reorderStrategies(IVault[] calldata newStrategiesOrder) external nonReentrant() notPaused() onlyAuth(STRATEGIST_ROLE) {
    VeryLiquidVaultStorage storage $ = _getVeryLiquidVaultStorage();
    uint256 length = $._strategies.length;
    if (length != newStrategiesOrder.length) revert ArrayLengthMismatch(length, newStrategiesOrder.length);
    for (uint256 i = 0; i < length; ++i) {
        if (!_isStrategy(newStrategiesOrder[i])) revert InvalidStrategy(address(newStrategiesOrder[i]));
        for (uint256 j = i + 1; j < length; ++j) {
            if (newStrategiesOrder[i] == newStrategiesOrder[j]) {
                revert InvalidStrategy(address(newStrategiesOrder[i]));
            }
        }
    }
    for (uint256 i = 0; i < length; ++i) {
        IVault strategyOld = $._strategies[i];
        $._strategies[i] = newStrategiesOrder[i];
        emit StrategyReordered(address(strategyOld), address(newStrategiesOrder[i]), i);
    }
}
```

## Related Implementations

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

## State Variable Reads

- **ENTERED** (`uint256`)
- **NOT_ENTERED** (`uint256`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: VeryLiquidVault.reorderStrategies(contract IVault[]) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: external
  ├─ [1] ⚙️ FUNCTION: VeryLiquidVault._getVeryLiquidVaultStorage() (NodeID: 1)
  │   💬 Args: [no args]
  │   👁️  Def: private
  ├─ [1] ⚙️ FUNCTION: VeryLiquidVault._isStrategy(contract IVault) (NodeID: 2)
  │   💬 Args: [newStrategiesOrder[i]]
  │   👁️  Def: private
  │ └─ [2] ⚙️ FUNCTION: VeryLiquidVault._getVeryLiquidVaultStorage() (NodeID: 3)
  │     💬 Args: [no args]
  │     👁️  Def: private
  ├─ [1] 🔒 MODIFIER: BaseVault.onlyAuth(bytes32) (NodeID: 4)
  │   💬 Args: [STRATEGIST_ROLE]
  │ └─ [2] ⚙️ FUNCTION: BaseVault._checkAuthRole(bytes32) (NodeID: 5)
  │     💬 Args: [STRATEGIST_ROLE]
  │     👁️  Def: internal
  │   ├─ [3] ⚙️ FUNCTION: BaseVault.auth() (NodeID: 6)
  │   │   💬 Args: [no args]
  │   │   👁️  Def: public
  │   │ └─ [4] ⚙️ FUNCTION: BaseVault._getBaseVaultStorage() (NodeID: 7)
  │   │     💬 Args: [no args]
  │   │     👁️  Def: private
  │   ├─ [3] ⚙️ FUNCTION: ContextUpgradeable._msgSender() (NodeID: 8)
  │   │   💬 Args: [no args]
  │   │   👁️  Def: internal
  │   └─ [3] ⚙️ FUNCTION: ContextUpgradeable._msgSender() (NodeID: 9)
  │       💬 Args: [no args]
  │       👁️  Def: internal
  ├─ [1] 🔒 MODIFIER: BaseVault.notPaused() (NodeID: 10)
  │   💬 Args: [no args]
  │ └─ [2] ⚙️ FUNCTION: BaseVault._requireNotPausedAuthNotPaused() (NodeID: 11)
  │     💬 Args: [no args]
  │     👁️  Def: internal
  │   └─ [3] ⚙️ FUNCTION: BaseVault._pausedOrAuthPaused() (NodeID: 12)
  │       💬 Args: [no args]
  │       👁️  Def: private
  │     ├─ [4] ⚙️ FUNCTION: BaseVault.auth() (NodeID: 13)
  │     │   💬 Args: [no args]
  │     │   👁️  Def: public
  │     │ └─ [5] ⚙️ FUNCTION: BaseVault._getBaseVaultStorage() (NodeID: 14)
  │     │     💬 Args: [no args]
  │     │     👁️  Def: private
  │     └─ [4] ⚙️ FUNCTION: PausableUpgradeable.paused() (NodeID: 15)
  │         💬 Args: [no args]
  │         👁️  Def: public
  │       └─ [5] ⚙️ FUNCTION: PausableUpgradeable._getPausableStorage() (NodeID: 16)
  │           💬 Args: [no args]
  │           👁️  Def: private
  └─ [1] 🔒 MODIFIER: ReentrancyGuardUpgradeable.nonReentrant() (NodeID: 17)
      💬 Args: [no args]
    ├─ [2] ⚙️ FUNCTION: ReentrancyGuardUpgradeable._nonReentrantBefore() (NodeID: 18)
    │   💬 Args: [no args]
    │   👁️  Def: private
    │ └─ [3] ⚙️ FUNCTION: ReentrancyGuardUpgradeable._getReentrancyGuardStorage() (NodeID: 19)
    │     💬 Args: [no args]
    │     👁️  Def: private
    └─ [2] ⚙️ FUNCTION: ReentrancyGuardUpgradeable._nonReentrantAfter() (NodeID: 20)
        💬 Args: [no args]
        👁️  Def: private
      └─ [3] ⚙️ FUNCTION: ReentrancyGuardUpgradeable._getReentrancyGuardStorage() (NodeID: 21)
          💬 Args: [no args]
          👁️  Def: private
```

## Documentation

### Function Documentation

@notice Reorders the strategies
 @param newStrategiesOrder The new strategies order
 @dev Only callable by addresses with STRATEGIST_ROLE
 @dev Verifies that the new strategies order is valid and that there are no duplicates
 @dev Clears current strategies and adds them in the new order
