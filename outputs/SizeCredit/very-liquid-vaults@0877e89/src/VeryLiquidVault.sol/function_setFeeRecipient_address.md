# Function: setFeeRecipient(address)

**Contract**: [src/VeryLiquidVault.sol/contract_VeryLiquidVault.md]

## Metadata

- **Contract**: VeryLiquidVault
- **Signature**: `setFeeRecipient(address)`
- **Visibility**: external
- **Source Range**: 11243:147:145

## Implementation

```solidity
/// @notice Sets the fee recipient
///  @param feeRecipient_ The new fee recipient
///  @dev Only callable by addresses with DEFAULT_ADMIN_ROLE
function setFeeRecipient(address feeRecipient_) external nonReentrant() onlyAuth(DEFAULT_ADMIN_ROLE) {
    _setFeeRecipient(feeRecipient_);
}
```

## Related Implementations

### _setFeeRecipient(address)

- **Kind**: internal
- **Source**: 3809:364:151
- **Link**: `src/utils/PerformanceVault.sol:PerformanceVault:_setFeeRecipient(address)`

```solidity
/// @notice Sets the fee recipient
///  @param feeRecipient_ The new fee recipient
function _setFeeRecipient(address feeRecipient_) internal {
    if (feeRecipient_ == address(0)) revert NullAddress();
    PerformanceVaultStorage storage $ = _getPerformanceVaultStorage();
    address feeRecipientBefore = $._feeRecipient;
    $._feeRecipient = feeRecipient_;
    emit FeeRecipientSet(feeRecipientBefore, feeRecipient_);
}
```

### _getPerformanceVaultStorage()

- **Kind**: internal
- **Source**: 1114:186:151
- **Link**: `src/utils/PerformanceVault.sol:PerformanceVault:_getPerformanceVaultStorage()`

```solidity
function _getPerformanceVaultStorage() private pure returns (PerformanceVaultStorage storage $) {
    assembly {
        $.slot := PerformanceVaultStorageLocation
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
┌─ [0] ⚙️ FUNCTION: VeryLiquidVault.setFeeRecipient(address) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: external
  ├─ [1] ⚙️ FUNCTION: PerformanceVault._setFeeRecipient(address) (NodeID: 1)
  │   💬 Args: [feeRecipient_]
  │   👁️  Def: internal
  │ └─ [2] ⚙️ FUNCTION: PerformanceVault._getPerformanceVaultStorage() (NodeID: 2)
  │     💬 Args: [no args]
  │     👁️  Def: private
  ├─ [1] 🔒 MODIFIER: BaseVault.onlyAuth(bytes32) (NodeID: 3)
  │   💬 Args: [DEFAULT_ADMIN_ROLE]
  │ └─ [2] ⚙️ FUNCTION: BaseVault._checkAuthRole(bytes32) (NodeID: 4)
  │     💬 Args: [DEFAULT_ADMIN_ROLE]
  │     👁️  Def: internal
  │   ├─ [3] ⚙️ FUNCTION: BaseVault.auth() (NodeID: 5)
  │   │   💬 Args: [no args]
  │   │   👁️  Def: public
  │   │ └─ [4] ⚙️ FUNCTION: BaseVault._getBaseVaultStorage() (NodeID: 6)
  │   │     💬 Args: [no args]
  │   │     👁️  Def: private
  │   ├─ [3] ⚙️ FUNCTION: ContextUpgradeable._msgSender() (NodeID: 7)
  │   │   💬 Args: [no args]
  │   │   👁️  Def: internal
  │   └─ [3] ⚙️ FUNCTION: ContextUpgradeable._msgSender() (NodeID: 8)
  │       💬 Args: [no args]
  │       👁️  Def: internal
  └─ [1] 🔒 MODIFIER: ReentrancyGuardUpgradeable.nonReentrant() (NodeID: 9)
      💬 Args: [no args]
    ├─ [2] ⚙️ FUNCTION: ReentrancyGuardUpgradeable._nonReentrantBefore() (NodeID: 10)
    │   💬 Args: [no args]
    │   👁️  Def: private
    │ └─ [3] ⚙️ FUNCTION: ReentrancyGuardUpgradeable._getReentrancyGuardStorage() (NodeID: 11)
    │     💬 Args: [no args]
    │     👁️  Def: private
    └─ [2] ⚙️ FUNCTION: ReentrancyGuardUpgradeable._nonReentrantAfter() (NodeID: 12)
        💬 Args: [no args]
        👁️  Def: private
      └─ [3] ⚙️ FUNCTION: ReentrancyGuardUpgradeable._getReentrancyGuardStorage() (NodeID: 13)
          💬 Args: [no args]
          👁️  Def: private
```

## Documentation

### Function Documentation

@notice Sets the fee recipient
 @param feeRecipient_ The new fee recipient
 @dev Only callable by addresses with DEFAULT_ADMIN_ROLE
