# Function: grantRole(bytes32,address)

**Contract**: [src/Auth.sol/contract_Auth.md]

## Metadata

- **Contract**: Auth
- **Signature**: `grantRole(bytes32,address)`
- **Visibility**: public
- **Source Range**: 5246:136:54
- **Inherited From**: AccessControlUpgradeable

## Implementation

```solidity
///  @dev Grants `role` to `account`.
///  If `account` had not been already granted `role`, emits a {RoleGranted}
///  event.
///  Requirements:
///  - the caller must have ``role``'s admin role.
///  May emit a {RoleGranted} event.
function grantRole(bytes32 role, address account) virtual public onlyRole(getRoleAdmin(role)) {
    _grantRole(role, account);
}
```

## Related Implementations

### _grantRole(bytes32,address)

- **Kind**: internal
- **Source**: 3987:348:55
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/access/extensions/AccessControlEnumerableUpgradeable.sol:AccessControlEnumerableUpgradeable:_grantRole(bytes32,address)`

```solidity
///  @dev Overload {AccessControl-_grantRole} to track enumerable memberships
function _grantRole(bytes32 role, address account) virtual override internal returns (bool) {
    AccessControlEnumerableStorage storage $ = _getAccessControlEnumerableStorage();
    bool granted = super._grantRole(role, account);
    if (granted) {
        $._roleMembers[role].add(account);
    }
    return granted;
}
```

### _getAccessControlEnumerableStorage()

- **Kind**: internal
- **Source**: 1250:207:55
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/access/extensions/AccessControlEnumerableUpgradeable.sol:AccessControlEnumerableUpgradeable:_getAccessControlEnumerableStorage()`

```solidity
function _getAccessControlEnumerableStorage() private pure returns (AccessControlEnumerableStorage storage $) {
    assembly {
        $.slot := AccessControlEnumerableStorageLocation
    }
}
```

### _grantRole(bytes32,address)

- **Kind**: internal
- **Source**: 7270:387:54
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/access/AccessControlUpgradeable.sol:AccessControlUpgradeable:_grantRole(bytes32,address)`

```solidity
///  @dev Attempts to grant `role` to `account` and returns a boolean indicating if `role` was granted.
///  Internal function without access restriction.
///  May emit a {RoleGranted} event.
function _grantRole(bytes32 role, address account) virtual internal returns (bool) {
    AccessControlStorage storage $ = _getAccessControlStorage();
    if (!hasRole(role, account)) {
        $._roles[role].hasRole[account] = true;
        emit RoleGranted(role, account, _msgSender());
        return true;
    } else {
        return false;
    }
}
```

### _getAccessControlStorage()

- **Kind**: internal
- **Source**: 2787:177:54
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
- **Source**: 3732:207:54
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
- **Source**: 887:96:61
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/utils/ContextUpgradeable.sol:ContextUpgradeable:_msgSender()`

```solidity
function _msgSender() virtual internal view returns (address) {
    return msg.sender;
}
```

### add(struct EnumerableSet.AddressSet,address)

- **Kind**: internal
- **Source**: 9332:150:112
- **Link**: `lib/openzeppelin-contracts/contracts/utils/structs/EnumerableSet.sol:EnumerableSet:add(struct EnumerableSet.AddressSet,address)`

```solidity
///  @dev Add a value to a set. O(1).
///  Returns true if the value was added to the set, that is if it was not
///  already present.
function add(AddressSet storage set, address value) internal returns (bool) {
    return _add(set._inner, bytes32(uint256(uint160(value))));
}
```

### _add(struct EnumerableSet.Set,bytes32)

- **Kind**: internal
- **Source**: 2336:406:112
- **Link**: `lib/openzeppelin-contracts/contracts/utils/structs/EnumerableSet.sol:EnumerableSet:_add(struct EnumerableSet.Set,bytes32)`

```solidity
///  @dev Add a value to a set. O(1).
///  Returns true if the value was added to the set, that is if it was not
///  already present.
function _add(Set storage set, bytes32 value) private returns (bool) {
    if (!_contains(set, value)) {
        set._values.push(value);
        set._positions[value] = set._values.length;
        return true;
    } else {
        return false;
    }
}
```

### _contains(struct EnumerableSet.Set,bytes32)

- **Kind**: internal
- **Source**: 4910:129:112
- **Link**: `lib/openzeppelin-contracts/contracts/utils/structs/EnumerableSet.sol:EnumerableSet:_contains(struct EnumerableSet.Set,bytes32)`

```solidity
///  @dev Returns true if the value is in the set. O(1).
function _contains(Set storage set, bytes32 value) private view returns (bool) {
    return set._positions[value] != 0;
}
```

### onlyRole(bytes32)

- **Kind**: modifier
- **Source**: 3149:76:54
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
- **Source**: 4759:191:54
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
- **Source**: 4148:103:54
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
- **Source**: 4381:197:54
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
┌─ [0] ⚙️ FUNCTION: AccessControlUpgradeable.grantRole(bytes32,address) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
  ├─ [1] ⚙️ FUNCTION: AccessControlEnumerableUpgradeable._grantRole(bytes32,address) (NodeID: 1)
  │   💬 Args: [role, account]
  │   👁️  Def: internal
  │ ├─ [2] ⚙️ FUNCTION: AccessControlEnumerableUpgradeable._getAccessControlEnumerableStorage() (NodeID: 2)
  │ │   💬 Args: [no args]
  │ │   👁️  Def: private
  │ ├─ [2] ⚙️ FUNCTION: AccessControlUpgradeable._grantRole(bytes32,address) (NodeID: 3)
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
  │ └─ [2] ⚙️ FUNCTION: EnumerableSet.add(struct EnumerableSet.AddressSet,address) (NodeID: 8)
  │     💬 Args: [$._roleMembers[role], account]
  │     👁️  Def: internal
  │   └─ [3] ⚙️ FUNCTION: EnumerableSet._add(struct EnumerableSet.Set,bytes32) (NodeID: 9)
  │       💬 Args: [set._inner, bytes32(uint256(uint160(value)))]
  │       👁️  Def: private
  │     └─ [4] ⚙️ FUNCTION: EnumerableSet._contains(struct EnumerableSet.Set,bytes32) (NodeID: 10)
  │         💬 Args: [set, value]
  │         👁️  Def: private
  └─ [1] 🔒 MODIFIER: AccessControlUpgradeable.onlyRole(bytes32) (NodeID: 11)
      💬 Args: [getRoleAdmin(role)]
    ├─ [2] ⚙️ FUNCTION: AccessControlUpgradeable.getRoleAdmin(bytes32) (NodeID: 12)
    │   💬 Args: [role]
    │   👁️  Def: public
    │ └─ [3] ⚙️ FUNCTION: AccessControlUpgradeable._getAccessControlStorage() (NodeID: 13)
    │     💬 Args: [no args]
    │     👁️  Def: private
    └─ [2] ⚙️ FUNCTION: AccessControlUpgradeable._checkRole(bytes32) (NodeID: 14)
        💬 Args: [getRoleAdmin(role)]
        👁️  Def: internal
      └─ [3] ⚙️ FUNCTION: AccessControlUpgradeable._checkRole(bytes32,address) (NodeID: 15)
          💬 Args: [role, _msgSender()]
          👁️  Def: internal
        ├─ [4] ⚙️ FUNCTION: ContextUpgradeable._msgSender() (NodeID: 18)
        │   💬 Args: [no args]
        │   👁️  Def: internal
        └─ [4] ⚙️ FUNCTION: AccessControlUpgradeable.hasRole(bytes32,address) (NodeID: 16)
            💬 Args: [role, account]
            👁️  Def: public
          └─ [5] ⚙️ FUNCTION: AccessControlUpgradeable._getAccessControlStorage() (NodeID: 17)
              💬 Args: [no args]
              👁️  Def: private
```

## Documentation

### Function Documentation

 @dev Grants `role` to `account`.
 If `account` had not been already granted `role`, emits a {RoleGranted}
 event.
 Requirements:
 - the caller must have ``role``'s admin role.
 May emit a {RoleGranted} event.

### Interface Documentation

 @dev Grants `role` to `account`.
 If `account` had not been already granted `role`, emits a {RoleGranted}
 event.
 Requirements:
 - the caller must have ``role``'s admin role.
