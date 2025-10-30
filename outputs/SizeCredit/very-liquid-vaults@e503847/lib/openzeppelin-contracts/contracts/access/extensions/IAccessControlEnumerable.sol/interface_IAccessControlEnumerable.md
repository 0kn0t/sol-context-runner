# Interface: IAccessControlEnumerable

## Metadata

- **Name**: IAccessControlEnumerable
- **Type**: Interface
- **Path**: lib/openzeppelin-contracts/contracts/access/extensions/IAccessControlEnumerable.sol
- **Documentation**:  @dev External interface of AccessControlEnumerable declared to support ERC-165 detection.

## Implements Interfaces

- **IAccessControl** [lib/openzeppelin-contracts/contracts/access/IAccessControl.sol/interface_IAccessControl.md]

## Errors

### AccessControlUnauthorizedAccount (inherited from IAccessControl)

```solidity
///  @dev The `account` is missing a role.
error AccessControlUnauthorizedAccount(address account, bytes32 neededRole);
```

### AccessControlBadConfirmation (inherited from IAccessControl)

```solidity
///  @dev The caller of a function is not the expected one.
///  NOTE: Don't confuse with {AccessControlUnauthorizedAccount}.
error AccessControlBadConfirmation();
```

## Events

### RoleAdminChanged (inherited from IAccessControl)

```solidity
///  @dev Emitted when `newAdminRole` is set as ``role``'s admin role, replacing `previousAdminRole`
///  `DEFAULT_ADMIN_ROLE` is the starting admin for all roles, despite
///  {RoleAdminChanged} not being emitted to signal this.
event RoleAdminChanged(bytes32 indexed role, bytes32 indexed previousAdminRole, bytes32 indexed newAdminRole);
```

### RoleGranted (inherited from IAccessControl)

```solidity
///  @dev Emitted when `account` is granted `role`.
///  `sender` is the account that originated the contract call. This account bears the admin role (for the granted role).
///  Expected in cases where the role was granted using the internal {AccessControl-_grantRole}.
event RoleGranted(bytes32 indexed role, address indexed account, address indexed sender);
```

### RoleRevoked (inherited from IAccessControl)

```solidity
///  @dev Emitted when `account` is revoked `role`.
///  `sender` is the account that originated the contract call:
///    - if using `revokeRole`, it is the admin role bearer
///    - if using `renounceRole`, it is the role bearer (i.e. `account`)
event RoleRevoked(bytes32 indexed role, address indexed account, address indexed sender);
```

## Public/External Functions

### getRoleMember(bytes32,uint256)

- **Signature**: `getRoleMember(bytes32,uint256)`
- **Visibility**: external
- **Source Range**: 950:84:26

**Signature:**
```solidity
///  @dev Returns one of the accounts that have `role`. `index` must be a
///  value between 0 and {getRoleMemberCount}, non-inclusive.
///  Role bearers are not sorted in any particular way, and their ordering may
///  change at any point.
///  WARNING: When using {getRoleMember} and {getRoleMemberCount}, make sure
///  you perform all queries on the same block. See the following
///  https://forum.openzeppelin.com/t/iterating-over-elements-on-enumerableset-in-openzeppelin-contracts/2296[forum post]
///  for more information.
function getRoleMember(bytes32 role, uint256 index) external view returns (address);;
```

### getRoleMemberCount(bytes32)

- **Signature**: `getRoleMemberCount(bytes32)`
- **Visibility**: external
- **Source Range**: 1202:74:26

**Signature:**
```solidity
///  @dev Returns the number of accounts that have `role`. Can be used
///  together with {getRoleMember} to enumerate all bearers of a role.
function getRoleMemberCount(bytes32 role) external view returns (uint256);;
```

### hasRole(bytes32,address) (inherited from IAccessControl)

- **Signature**: `hasRole(bytes32,address)`
- **Visibility**: external
- **Source Range**: 1822:77:25

**Signature:**
```solidity
///  @dev Returns `true` if `account` has been granted `role`.
function hasRole(bytes32 role, address account) external view returns (bool);;
```

### getRoleAdmin(bytes32) (inherited from IAccessControl)

- **Signature**: `getRoleAdmin(bytes32)`
- **Visibility**: external
- **Source Range**: 2094:68:25

**Signature:**
```solidity
///  @dev Returns the admin role that controls `role`. See {grantRole} and
///  {revokeRole}.
///  To change a role's admin, use {AccessControl-_setRoleAdmin}.
function getRoleAdmin(bytes32 role) external view returns (bytes32);;
```

### grantRole(bytes32,address) (inherited from IAccessControl)

- **Signature**: `grantRole(bytes32,address)`
- **Visibility**: external
- **Source Range**: 2412:59:25

**Signature:**
```solidity
///  @dev Grants `role` to `account`.
///  If `account` had not been already granted `role`, emits a {RoleGranted}
///  event.
///  Requirements:
///  - the caller must have ``role``'s admin role.
function grantRole(bytes32 role, address account) external;;
```

### revokeRole(bytes32,address) (inherited from IAccessControl)

- **Signature**: `revokeRole(bytes32,address)`
- **Visibility**: external
- **Source Range**: 2705:60:25

**Signature:**
```solidity
///  @dev Revokes `role` from `account`.
///  If `account` had been granted `role`, emits a {RoleRevoked} event.
///  Requirements:
///  - the caller must have ``role``'s admin role.
function revokeRole(bytes32 role, address account) external;;
```

### renounceRole(bytes32,address) (inherited from IAccessControl)

- **Signature**: `renounceRole(bytes32,address)`
- **Visibility**: external
- **Source Range**: 3267:73:25

**Signature:**
```solidity
///  @dev Revokes `role` from the calling account.
///  Roles are often managed via {grantRole} and {revokeRole}: this function's
///  purpose is to provide a mechanism for accounts to lose their privileges
///  if they are compromised (such as when a trusted device is misplaced).
///  If the calling account had been granted `role`, emits a {RoleRevoked}
///  event.
///  Requirements:
///  - the caller must be `callerConfirmation`.
function renounceRole(bytes32 role, address callerConfirmation) external;;
```
