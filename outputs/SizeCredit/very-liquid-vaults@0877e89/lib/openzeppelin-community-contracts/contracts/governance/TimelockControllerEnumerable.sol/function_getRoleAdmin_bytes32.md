# Function: getRoleAdmin(bytes32)

**Contract**: [lib/openzeppelin-community-contracts/contracts/governance/TimelockControllerEnumerable.sol/contract_TimelockControllerEnumerable.md]

## Metadata

- **Contract**: TimelockControllerEnumerable
- **Signature**: `getRoleAdmin(bytes32)`
- **Visibility**: public
- **Source Range**: 3810:120:68
- **Inherited From**: AccessControl

## Implementation

```solidity
///  @dev Returns the admin role that controls `role`. See {grantRole} and
///  {revokeRole}.
///  To change a role's admin, use {_setRoleAdmin}.
function getRoleAdmin(bytes32 role) virtual public view returns (bytes32) {
    return _roles[role].adminRole;
}
```

## State Variable Reads

- **_roles** (`mapping(bytes32 => struct AccessControl.RoleData)`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: AccessControl.getRoleAdmin(bytes32) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
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
