# Function: getRoleAdmin(bytes32)

**Contract**: [src/utils/Auth.sol/contract_Auth.md]

## Metadata

- **Contract**: Auth
- **Signature**: `getRoleAdmin(bytes32)`
- **Visibility**: public
- **Source Range**: 4759:191:11
- **Inherited From**: AccessControlUpgradeable

## Implementation

```solidity
///  @dev Returns the admin role that controls `role`. See {grantRole} and
///  {revokeRole}.
///  To change a role's admin, use {_setRoleAdmin}.
function getRoleAdmin(bytes32 role) virtual public view returns (bytes32) {
    AccessControlStorage storage $ = _getAccessControlStorage();
    return $._roles[role].adminRole;
}
```

## Related Implementations

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

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: AccessControlUpgradeable.getRoleAdmin(bytes32) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
  └─ [1] ⚙️ FUNCTION: AccessControlUpgradeable._getAccessControlStorage() (NodeID: 1)
      💬 Args: [no args]
      👁️  Def: private
```

## Documentation

### Function Documentation

 @dev Returns the admin role that controls `role`. See {grantRole} and
 {revokeRole}.
 To change a role's admin, use {_setRoleAdmin}.

### Interface Documentation

 @dev Returns the admin role that controls `role`. See {grantRole} and
 {revokeRole}.
 To change a role's admin, use {AccessControl-_setRoleAdmin}.
