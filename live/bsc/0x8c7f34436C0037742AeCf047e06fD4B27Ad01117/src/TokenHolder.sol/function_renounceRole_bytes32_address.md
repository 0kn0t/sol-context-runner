# Function: renounceRole(bytes32,address)

**Contract**: [src/TokenHolder.sol/contract_TokenHolder.md]

## Metadata

- **Contract**: TokenHolder
- **Signature**: `renounceRole(bytes32,address)`
- **Visibility**: public
- **Source Range**: 6417:245:0
- **Inherited From**: AccessControlUpgradeable

## Implementation

```solidity
///  @dev Revokes `role` from the calling account.
///  Roles are often managed via {grantRole} and {revokeRole}: this function's
///  purpose is to provide a mechanism for accounts to lose their privileges
///  if they are compromised (such as when a trusted device is misplaced).
///  If the calling account had been revoked `role`, emits a {RoleRevoked}
///  event.
///  Requirements:
///  - the caller must be `callerConfirmation`.
///  May emit a {RoleRevoked} event.
function renounceRole(bytes32 role, address callerConfirmation) virtual public {
    if (callerConfirmation != _msgSender()) {
        revert AccessControlBadConfirmation();
    }
    _revokeRole(role, callerConfirmation);
}
```

## Related Implementations

### _msgSender()

- **Kind**: internal
- **Source**: 908:96:2
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/utils/ContextUpgradeable.sol:ContextUpgradeable:_msgSender()`

```solidity
function _msgSender() virtual internal view returns (address) {
    return msg.sender;
}
```

### _revokeRole(bytes32,address)

- **Kind**: internal
- **Source**: 7963:388:0
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/access/AccessControlUpgradeable.sol:AccessControlUpgradeable:_revokeRole(bytes32,address)`

```solidity
///  @dev Attempts to revoke `role` from `account` and returns a boolean indicating if `role` was revoked.
///  Internal function without access restriction.
///  May emit a {RoleRevoked} event.
function _revokeRole(bytes32 role, address account) virtual internal returns (bool) {
    AccessControlStorage storage $ = _getAccessControlStorage();
    if (hasRole(role, account)) {
        $._roles[role].hasRole[account] = false;
        emit RoleRevoked(role, account, _msgSender());
        return true;
    } else {
        return false;
    }
}
```

### _getAccessControlStorage()

- **Kind**: internal
- **Source**: 2889:177:0
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/access/AccessControlUpgradeable.sol:AccessControlUpgradeable:_getAccessControlStorage()`

```solidity
function _getAccessControlStorage() private pure returns (AccessControlStorage storage $) {
    assembly {
        $.slot := AccessControlStorageLocation
    }
}
```

### hasRole(bytes32,address)

- **Kind**: internal
- **Source**: 3801:207:0
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/access/AccessControlUpgradeable.sol:AccessControlUpgradeable:hasRole(bytes32,address)`

```solidity
///  @dev Returns `true` if `account` has been granted `role`.
function hasRole(bytes32 role, address account) virtual public view returns (bool) {
    AccessControlStorage storage $ = _getAccessControlStorage();
    return $._roles[role].hasRole[account];
}
```

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: AccessControlUpgradeable.renounceRole(bytes32,address) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
  ├─ [1] ⚙️ FUNCTION: ContextUpgradeable._msgSender() (NodeID: 1)
  │   💬 Args: [no args]
  │   👁️  Def: internal
  └─ [1] ⚙️ FUNCTION: AccessControlUpgradeable._revokeRole(bytes32,address) (NodeID: 2)
      💬 Args: [role, callerConfirmation]
      👁️  Def: internal
    ├─ [2] ⚙️ FUNCTION: AccessControlUpgradeable._getAccessControlStorage() (NodeID: 3)
    │   💬 Args: [no args]
    │   👁️  Def: private
    ├─ [2] ⚙️ FUNCTION: AccessControlUpgradeable.hasRole(bytes32,address) (NodeID: 4)
    │   💬 Args: [role, account]
    │   👁️  Def: public
    │ └─ [3] ⚙️ FUNCTION: AccessControlUpgradeable._getAccessControlStorage() (NodeID: 5)
    │     💬 Args: [no args]
    │     👁️  Def: private
    └─ [2] ⚙️ FUNCTION: ContextUpgradeable._msgSender() (NodeID: 6)
        💬 Args: [no args]
        👁️  Def: internal
```

## Documentation

### Function Documentation

 @dev Revokes `role` from the calling account.
 Roles are often managed via {grantRole} and {revokeRole}: this function's
 purpose is to provide a mechanism for accounts to lose their privileges
 if they are compromised (such as when a trusted device is misplaced).
 If the calling account had been revoked `role`, emits a {RoleRevoked}
 event.
 Requirements:
 - the caller must be `callerConfirmation`.
 May emit a {RoleRevoked} event.

### Interface Documentation

 @dev Revokes `role` from the calling account.
 Roles are often managed via {grantRole} and {revokeRole}: this function's
 purpose is to provide a mechanism for accounts to lose their privileges
 if they are compromised (such as when a trusted device is misplaced).
 If the calling account had been granted `role`, emits a {RoleRevoked}
 event.
 Requirements:
 - the caller must be `callerConfirmation`.
