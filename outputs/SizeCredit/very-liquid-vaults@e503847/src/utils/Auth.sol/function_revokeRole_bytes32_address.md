# Function: revokeRole(bytes32,address)

**Contract**: [src/utils/Auth.sol/contract_Auth.md]

## Metadata

- **Contract**: Auth
- **Signature**: `revokeRole(bytes32,address)`
- **Visibility**: public
- **Source Range**: 5662:138:11
- **Inherited From**: AccessControlUpgradeable

## Implementation

```solidity
///  @dev Revokes `role` from `account`.
///  If `account` had been granted `role`, emits a {RoleRevoked} event.
///  Requirements:
///  - the caller must have ``role``'s admin role.
///  May emit a {RoleRevoked} event.
function revokeRole(bytes32 role, address account) virtual public onlyRole(getRoleAdmin(role)) {
    _revokeRole(role, account);
}
```

## Related Implementations

### _revokeRole(bytes32,address)

- **Kind**: internal
- **Source**: 4438:353:12
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/access/extensions/AccessControlEnumerableUpgradeable.sol:AccessControlEnumerableUpgradeable:_revokeRole(bytes32,address)`

```solidity
///  @dev Overload {AccessControl-_revokeRole} to track enumerable memberships
function _revokeRole(bytes32 role, address account) virtual override internal returns (bool) {
    AccessControlEnumerableStorage storage $ = _getAccessControlEnumerableStorage();
    bool revoked = super._revokeRole(role, account);
    if (revoked) {
        $._roleMembers[role].remove(account);
    }
    return revoked;
}
```

### _getAccessControlEnumerableStorage()

- **Kind**: internal
- **Source**: 1250:207:12
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/access/extensions/AccessControlEnumerableUpgradeable.sol:AccessControlEnumerableUpgradeable:_getAccessControlEnumerableStorage()`

```solidity
function _getAccessControlEnumerableStorage() private pure returns (AccessControlEnumerableStorage storage $) {
    assembly {
        $.slot := AccessControlEnumerableStorageLocation
    }
}
```

### _revokeRole(bytes32,address)

- **Kind**: internal
- **Source**: 7894:388:11
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
- **Source**: 2787:177:11
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
- **Source**: 3732:207:11
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/access/AccessControlUpgradeable.sol:AccessControlUpgradeable:hasRole(bytes32,address)`

```solidity
///  @dev Returns `true` if `account` has been granted `role`.
function hasRole(bytes32 role, address account) virtual public view returns (bool) {
    AccessControlStorage storage $ = _getAccessControlStorage();
    return $._roles[role].hasRole[account];
}
```

### _msgSender()

- **Kind**: internal
- **Source**: 887:96:18
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/utils/ContextUpgradeable.sol:ContextUpgradeable:_msgSender()`

```solidity
function _msgSender() virtual internal view returns (address) {
    return msg.sender;
}
```

### remove(struct EnumerableSet.AddressSet,address)

- **Kind**: internal
- **Source**: 9650:156:55
- **Link**: `lib/openzeppelin-contracts/contracts/utils/structs/EnumerableSet.sol:EnumerableSet:remove(struct EnumerableSet.AddressSet,address)`

```solidity
///  @dev Removes a value from a set. O(1).
///  Returns true if the value was removed from the set, that is if it was
///  present.
function remove(AddressSet storage set, address value) internal returns (bool) {
    return _remove(set._inner, bytes32(uint256(uint160(value))));
}
```

### _remove(struct EnumerableSet.Set,bytes32)

- **Kind**: internal
- **Source**: 2910:1368:55
- **Link**: `lib/openzeppelin-contracts/contracts/utils/structs/EnumerableSet.sol:EnumerableSet:_remove(struct EnumerableSet.Set,bytes32)`

```solidity
///  @dev Removes a value from a set. O(1).
///  Returns true if the value was removed from the set, that is if it was
///  present.
function _remove(Set storage set, bytes32 value) private returns (bool) {
    uint256 position = set._positions[value];
    if (position != 0) {
        uint256 valueIndex = position - 1;
        uint256 lastIndex = set._values.length - 1;
        if (valueIndex != lastIndex) {
            bytes32 lastValue = set._values[lastIndex];
            set._values[valueIndex] = lastValue;
            set._positions[lastValue] = position;
        }
        set._values.pop();
        delete set._positions[value];
        return true;
    } else {
        return false;
    }
}
```

### onlyRole(bytes32)

- **Kind**: modifier
- **Source**: 3149:76:11
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/access/AccessControlUpgradeable.sol:AccessControlUpgradeable:onlyRole(bytes32)`

```solidity
///  @dev Modifier that checks that an account has a specific role. Reverts
///  with an {AccessControlUnauthorizedAccount} error including the required role.
modifier onlyRole(bytes32 role) {
    _checkRole(role);
    _;
}
```

### getRoleAdmin(bytes32)

- **Kind**: internal
- **Source**: 4759:191:11
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/access/AccessControlUpgradeable.sol:AccessControlUpgradeable:getRoleAdmin(bytes32)`

```solidity
///  @dev Returns the admin role that controls `role`. See {grantRole} and
///  {revokeRole}.
///  To change a role's admin, use {_setRoleAdmin}.
function getRoleAdmin(bytes32 role) virtual public view returns (bytes32) {
    AccessControlStorage storage $ = _getAccessControlStorage();
    return $._roles[role].adminRole;
}
```

### _checkRole(bytes32)

- **Kind**: internal
- **Source**: 4148:103:11
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/access/AccessControlUpgradeable.sol:AccessControlUpgradeable:_checkRole(bytes32)`

```solidity
///  @dev Reverts with an {AccessControlUnauthorizedAccount} error if `_msgSender()`
///  is missing `role`. Overriding this function changes the behavior of the {onlyRole} modifier.
function _checkRole(bytes32 role) virtual internal view {
    _checkRole(role, _msgSender());
}
```

### _checkRole(bytes32,address)

- **Kind**: internal
- **Source**: 4381:197:11
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/access/AccessControlUpgradeable.sol:AccessControlUpgradeable:_checkRole(bytes32,address)`

```solidity
///  @dev Reverts with an {AccessControlUnauthorizedAccount} error if `account`
///  is missing `role`.
function _checkRole(bytes32 role, address account) virtual internal view {
    if (!hasRole(role, account)) {
        revert AccessControlUnauthorizedAccount(account, role);
    }
}
```

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: AccessControlUpgradeable.revokeRole(bytes32,address) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
  ├─ [1] ⚙️ FUNCTION: AccessControlEnumerableUpgradeable._revokeRole(bytes32,address) (NodeID: 1)
  │   💬 Args: [role, account]
  │   👁️  Def: internal
  │ ├─ [2] ⚙️ FUNCTION: AccessControlEnumerableUpgradeable._getAccessControlEnumerableStorage() (NodeID: 2)
  │ │   💬 Args: [no args]
  │ │   👁️  Def: private
  │ ├─ [2] ⚙️ FUNCTION: AccessControlUpgradeable._revokeRole(bytes32,address) (NodeID: 3)
  │ │   💬 Args: [role, account]
  │ │   👁️  Def: internal
  │ │ ├─ [3] ⚙️ FUNCTION: AccessControlUpgradeable._getAccessControlStorage() (NodeID: 4)
  │ │ │   💬 Args: [no args]
  │ │ │   👁️  Def: private
  │ │ ├─ [3] ⚙️ FUNCTION: AccessControlUpgradeable.hasRole(bytes32,address) (NodeID: 5)
  │ │ │   💬 Args: [role, account]
  │ │ │   👁️  Def: public
  │ │ │ └─ [4] ⚙️ FUNCTION: AccessControlUpgradeable._getAccessControlStorage() (NodeID: 6)
  │ │ │     💬 Args: [no args]
  │ │ │     👁️  Def: private
  │ │ └─ [3] ⚙️ FUNCTION: ContextUpgradeable._msgSender() (NodeID: 7)
  │ │     💬 Args: [no args]
  │ │     👁️  Def: internal
  │ └─ [2] ⚙️ FUNCTION: EnumerableSet.remove(struct EnumerableSet.AddressSet,address) (NodeID: 8)
  │     💬 Args: [$._roleMembers[role], account]
  │     👁️  Def: internal
  │   └─ [3] ⚙️ FUNCTION: EnumerableSet._remove(struct EnumerableSet.Set,bytes32) (NodeID: 9)
  │       💬 Args: [set._inner, bytes32(uint256(uint160(value)))]
  │       👁️  Def: private
  └─ [1] 🔒 MODIFIER: AccessControlUpgradeable.onlyRole(bytes32) (NodeID: 10)
      💬 Args: [getRoleAdmin(role)]
    ├─ [2] ⚙️ FUNCTION: AccessControlUpgradeable.getRoleAdmin(bytes32) (NodeID: 11)
    │   💬 Args: [role]
    │   👁️  Def: public
    │ └─ [3] ⚙️ FUNCTION: AccessControlUpgradeable._getAccessControlStorage() (NodeID: 12)
    │     💬 Args: [no args]
    │     👁️  Def: private
    └─ [2] ⚙️ FUNCTION: AccessControlUpgradeable._checkRole(bytes32) (NodeID: 13)
        💬 Args: [getRoleAdmin(role)]
        👁️  Def: internal
      └─ [3] ⚙️ FUNCTION: AccessControlUpgradeable._checkRole(bytes32,address) (NodeID: 14)
          💬 Args: [role, _msgSender()]
          👁️  Def: internal
        ├─ [4] ⚙️ FUNCTION: ContextUpgradeable._msgSender() (NodeID: 17)
        │   💬 Args: [no args]
        │   👁️  Def: internal
        └─ [4] ⚙️ FUNCTION: AccessControlUpgradeable.hasRole(bytes32,address) (NodeID: 15)
            💬 Args: [role, account]
            👁️  Def: public
          └─ [5] ⚙️ FUNCTION: AccessControlUpgradeable._getAccessControlStorage() (NodeID: 16)
              💬 Args: [no args]
              👁️  Def: private
```

## Documentation

### Function Documentation

 @dev Revokes `role` from `account`.
 If `account` had been granted `role`, emits a {RoleRevoked} event.
 Requirements:
 - the caller must have ``role``'s admin role.
 May emit a {RoleRevoked} event.

### Interface Documentation

 @dev Revokes `role` from `account`.
 If `account` had been granted `role`, emits a {RoleRevoked} event.
 Requirements:
 - the caller must have ``role``'s admin role.
