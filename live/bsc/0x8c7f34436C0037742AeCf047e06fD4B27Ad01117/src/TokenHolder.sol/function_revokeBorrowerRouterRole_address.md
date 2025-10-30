# Function: revokeBorrowerRouterRole(address)

**Contract**: [src/TokenHolder.sol/contract_TokenHolder.md]

## Metadata

- **Contract**: TokenHolder
- **Signature**: `revokeBorrowerRouterRole(address)`
- **Visibility**: public
- **Source Range**: 9653:160:13

## Implementation

```solidity
function revokeBorrowerRouterRole(address account) public onlyRole(DEFAULT_ADMIN_ROLE) {
    _revokeRole(BORROWER_ROUTER_ROLE, account);
}
```

## Related Implementations

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

### _msgSender()

- **Kind**: internal
- **Source**: 908:96:2
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/utils/ContextUpgradeable.sol:ContextUpgradeable:_msgSender()`

```solidity
function _msgSender() virtual internal view returns (address) {
    return msg.sender;
}
```

### onlyRole(bytes32)

- **Kind**: modifier
- **Source**: 3251:76:0
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/access/AccessControlUpgradeable.sol:AccessControlUpgradeable:onlyRole(bytes32)`

```solidity
///  @dev Modifier that checks that an account has a specific role. Reverts
///  with an {AccessControlUnauthorizedAccount} error including the required role.
modifier onlyRole(bytes32 role) {
    _checkRole(role);
    _;
}
```

### _checkRole(bytes32)

- **Kind**: internal
- **Source**: 4217:103:0
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
- **Source**: 4450:197:0
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

## State Variable Reads

- **BORROWER_ROUTER_ROLE** (`bytes32`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: TokenHolder.revokeBorrowerRouterRole(address) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
  ├─ [1] ⚙️ FUNCTION: AccessControlUpgradeable._revokeRole(bytes32,address) (NodeID: 1)
  │   💬 Args: [BORROWER_ROUTER_ROLE, account]
  │   👁️  Def: internal
  │ ├─ [2] ⚙️ FUNCTION: AccessControlUpgradeable._getAccessControlStorage() (NodeID: 2)
  │ │   💬 Args: [no args]
  │ │   👁️  Def: private
  │ ├─ [2] ⚙️ FUNCTION: AccessControlUpgradeable.hasRole(bytes32,address) (NodeID: 3)
  │ │   💬 Args: [role, account]
  │ │   👁️  Def: public
  │ │ └─ [3] ⚙️ FUNCTION: AccessControlUpgradeable._getAccessControlStorage() (NodeID: 4)
  │ │     💬 Args: [no args]
  │ │     👁️  Def: private
  │ └─ [2] ⚙️ FUNCTION: ContextUpgradeable._msgSender() (NodeID: 5)
  │     💬 Args: [no args]
  │     👁️  Def: internal
  └─ [1] 🔒 MODIFIER: AccessControlUpgradeable.onlyRole(bytes32) (NodeID: 6)
      💬 Args: [DEFAULT_ADMIN_ROLE]
    └─ [2] ⚙️ FUNCTION: AccessControlUpgradeable._checkRole(bytes32) (NodeID: 7)
        💬 Args: [DEFAULT_ADMIN_ROLE]
        👁️  Def: internal
      └─ [3] ⚙️ FUNCTION: AccessControlUpgradeable._checkRole(bytes32,address) (NodeID: 8)
          💬 Args: [role, _msgSender()]
          👁️  Def: internal
        ├─ [4] ⚙️ FUNCTION: ContextUpgradeable._msgSender() (NodeID: 11)
        │   💬 Args: [no args]
        │   👁️  Def: internal
        └─ [4] ⚙️ FUNCTION: AccessControlUpgradeable.hasRole(bytes32,address) (NodeID: 9)
            💬 Args: [role, account]
            👁️  Def: public
          └─ [5] ⚙️ FUNCTION: AccessControlUpgradeable._getAccessControlStorage() (NodeID: 10)
              💬 Args: [no args]
              👁️  Def: private
```
