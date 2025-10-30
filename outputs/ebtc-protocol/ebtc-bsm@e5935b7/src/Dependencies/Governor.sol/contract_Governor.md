# Contract: Governor

## Metadata

- **Name**: Governor
- **Type**: Contract
- **Path**: src/Dependencies/Governor.sol
- **Documentation**: @notice Role based Authority that supports up to 256 roles.
   @notice We have taken the tradeoff of additional storage usage for easier readabiliy without using off-chain / indexing services.
   @author BadgerDAO Expanded from Solmate RolesAuthority
   @author Modified from Solmate (https://github.com/transmissions11/solmate/blob/main/src/auth/authorities/RolesAuthority.sol)
   @author Modified from Dappsys (https://github.com/dapphub/ds-roles/blob/master/src/roles.sol)

## Implements Interfaces

- **Authority** [src/Dependencies/Authority.sol/interface_Authority.md]
- **IRolesAuthority** [src/Dependencies/IRolesAuthority.sol/interface_IRolesAuthority.md]

## State Variables

### owner (inherited from Auth)

```solidity
address public owner
```

### authority (inherited from Auth)

```solidity
Authority public authority
```

**Authority**: [src/Dependencies/Authority.sol/interface_Authority.md]

### users (inherited from RolesAuthority)

```solidity
EnumerableSet.AddressSet internal users
```

### targets (inherited from RolesAuthority)

```solidity
EnumerableSet.AddressSet internal targets
```

### enabledFunctionSigsByTarget (inherited from RolesAuthority)

```solidity
mapping(address => EnumerableSet.Bytes32Set) internal enabledFunctionSigsByTarget
```

### enabledFunctionSigsPublic (inherited from RolesAuthority)

```solidity
EnumerableSet.Bytes32Set internal enabledFunctionSigsPublic
```

### getUserRoles (inherited from RolesAuthority)

```solidity
mapping(address => bytes32) public getUserRoles
```

### capabilityFlag (inherited from RolesAuthority)

```solidity
mapping(address => mapping(bytes4 => CapabilityFlag)) public capabilityFlag
```

### getRolesWithCapability (inherited from RolesAuthority)

```solidity
mapping(address => mapping(bytes4 => bytes32)) public getRolesWithCapability
```

### NO_ROLES

```solidity
bytes32 internal NO_ROLES = bytes32(0)
```

### roleNames

```solidity
mapping(uint8 => string) internal roleNames
```

## Structs

### Role

```solidity
struct Role {
    uint8 roleId;
    string roleName;
}
```

### Capability

```solidity
struct Capability {
    address target;
    bytes4 functionSig;
    uint8[] roles;
}
```

## Events

### UserRoleUpdated (inherited from IRolesAuthority)

```solidity
event UserRoleUpdated(address indexed user, uint8 indexed role, bool enabled);
```

### PublicCapabilityUpdated (inherited from IRolesAuthority)

```solidity
event PublicCapabilityUpdated(address indexed target, bytes4 indexed functionSig, bool enabled);
```

### CapabilityBurned (inherited from IRolesAuthority)

```solidity
event CapabilityBurned(address indexed target, bytes4 indexed functionSig);
```

### RoleCapabilityUpdated (inherited from IRolesAuthority)

```solidity
event RoleCapabilityUpdated(uint8 indexed role, address indexed target, bytes4 indexed functionSig, bool enabled);
```

### OwnershipTransferred (inherited from Auth)

```solidity
event OwnershipTransferred(address indexed user, address indexed newOwner);
```

### AuthorityUpdated (inherited from Auth)

```solidity
event AuthorityUpdated(address indexed user, Authority indexed newAuthority);
```

### RoleNameSet

```solidity
event RoleNameSet(uint8 indexed role, string indexed name);
```

## Enums

### CapabilityFlag (inherited from IRolesAuthority)

```solidity
enum CapabilityFlag {
    None,
    Public,
    Burned
}
```

## Public/External Functions

### constructor(address)

- **Signature**: `constructor(address)`
- **Visibility**: public
- **Source Range**: 1350:79:28
- **Details**: [function_constructor_address.md](./function_constructor_address.md)

**Signature:**
```solidity
/// @notice The contract constructor initializes RolesAuthority with the given owner.
///  @param _owner The address of the owner, who gains all permissions by default.
constructor(address _owner) RolesAuthority(_owner,Authority(address(this)));
```

### getUsersByRole(uint8)

- **Signature**: `getUsersByRole(uint8)`
- **Visibility**: external
- **Source Range**: 1811:856:28
- **Details**: [function_getUsersByRole_uint8.md](./function_getUsersByRole_uint8.md)

**Signature:**
```solidity
/// @notice Returns a list of users that are assigned a specific role.
///  @dev This function searches all users and checks if they are assigned the given role.
///  @dev Intended for off-chain utility only due to inefficiency.
///  @param role The role ID to find users for.
///  @return usersWithRole An array of addresses that are assigned the given role.
function getUsersByRole(uint8 role) external view returns (address[] memory usersWithRole);
```

### getRolesForUser(address)

- **Signature**: `getRolesForUser(address)`
- **Visibility**: external
- **Source Range**: 3015:916:28
- **Details**: [function_getRolesForUser_address.md](./function_getRolesForUser_address.md)

**Signature:**
```solidity
/// @notice Returns a list of roles that an address has.
///  @dev This function searches all roles and checks if they are assigned to the given user.
///  @dev Intended for off-chain utility only due to inefficiency.
///  @param user The address of the user.
///  @return rolesForUser An array of role IDs that the user has.
function getRolesForUser(address user) external view returns (uint8[] memory rolesForUser);
```

### getRolesFromByteMap(bytes32)

- **Signature**: `getRolesFromByteMap(bytes32)`
- **Visibility**: public
- **Source Range**: 4148:946:28
- **Details**: [function_getRolesFromByteMap_bytes32.md](./function_getRolesFromByteMap_bytes32.md)

**Signature:**
```solidity
/// @notice Converts a byte map representation to an array of role IDs.
///  @param byteMap The bytes32 value encoding the roles.
///  @return roleIds An array of role IDs extracted from the byte map.
function getRolesFromByteMap(bytes32 byteMap) public pure returns (uint8[] memory roleIds);
```

### getByteMapFromRoles(uint8[])

- **Signature**: `getByteMapFromRoles(uint8[])`
- **Visibility**: public
- **Source Range**: 5273:256:28
- **Details**: [function_getByteMapFromRoles_uint8[].md](./function_getByteMapFromRoles_uint8[].md)

**Signature:**
```solidity
/// @notice Converts an array of role IDs to a byte map representation.
///  @param roleIds An array of role IDs.
///  @return A bytes32 value encoding the roles.
function getByteMapFromRoles(uint8[] memory roleIds) public pure returns (bytes32);
```

### getEnabledFunctionsInTarget(address)

- **Signature**: `getEnabledFunctionsInTarget(address)`
- **Visibility**: public
- **Source Range**: 5746:408:28
- **Details**: [function_getEnabledFunctionsInTarget_address.md](./function_getEnabledFunctionsInTarget_address.md)

**Signature:**
```solidity
/// @notice Retrieves all function signatures enabled for a target address.
///  @param _target The target contract address.
///  @return _funcs An array of function signatures enabled for the target.
function getEnabledFunctionsInTarget(address _target) public view returns (bytes4[] memory _funcs);
```

### getRoleName(uint8)

- **Signature**: `getRoleName(uint8)`
- **Visibility**: external
- **Source Range**: 6299:119:28
- **Details**: [function_getRoleName_uint8.md](./function_getRoleName_uint8.md)

**Signature:**
```solidity
/// @notice Retrieves the name associated with a role ID
///  @param role The role ID
///  @return roleName The name of the role
function getRoleName(uint8 role) external view returns (string memory roleName);
```

### setRoleName(uint8,string)

- **Signature**: `setRoleName(uint8,string)`
- **Visibility**: external
- **Source Range**: 6637:165:28
- **Details**: [function_setRoleName_uint8_string.md](./function_setRoleName_uint8_string.md)

**Signature:**
```solidity
/// @notice Sets the name for a specific role ID for better readability
///  @dev This function requires authorization
///  @param role The role ID
///  @param roleName The name to assign to the role
function setRoleName(uint8 role, string memory roleName) external requiresAuth();
```

### setAuthority(contract Authority) (inherited from Auth)

- **Signature**: `setAuthority(contract Authority)`
- **Visibility**: public
- **Source Range**: 1596:434:24
- **Details**: [function_setAuthority_contract_Authority.md](./function_setAuthority_contract_Authority.md)

**Signature:**
```solidity
function setAuthority(Authority newAuthority) virtual public;
```

### transferOwnership(address) (inherited from Auth)

- **Signature**: `transferOwnership(address)`
- **Visibility**: public
- **Source Range**: 2036:164:24
- **Details**: [function_transferOwnership_address.md](./function_transferOwnership_address.md)

**Signature:**
```solidity
function transferOwnership(address newOwner) virtual public requiresAuth();
```

### constructor(address,contract Authority) (inherited from RolesAuthority)

- **Signature**: `constructor(address,contract Authority)`
- **Visibility**: public
- **Source Range**: 867:77:37
- **Details**: [function_constructor_address_contract_Authority.md](./function_constructor_address_contract_Authority.md)

**Signature:**
```solidity
constructor(address _owner, Authority _authority) Auth(_owner,_authority);
```

### doesUserHaveRole(address,uint8) (inherited from RolesAuthority)

- **Signature**: `doesUserHaveRole(address,uint8)`
- **Visibility**: public
- **Source Range**: 1600:157:37
- **Details**: [function_doesUserHaveRole_address_uint8.md](./function_doesUserHaveRole_address_uint8.md)

**Signature:**
```solidity
function doesUserHaveRole(address user, uint8 role) virtual public view returns (bool);
```

### doesRoleHaveCapability(uint8,address,bytes4) (inherited from RolesAuthority)

- **Signature**: `doesRoleHaveCapability(uint8,address,bytes4)`
- **Visibility**: public
- **Source Range**: 1763:240:37
- **Details**: [function_doesRoleHaveCapability_uint8_address_bytes4.md](./function_doesRoleHaveCapability_uint8_address_bytes4.md)

**Signature:**
```solidity
function doesRoleHaveCapability(uint8 role, address target, bytes4 functionSig) virtual public view returns (bool);
```

### isPublicCapability(address,bytes4) (inherited from RolesAuthority)

- **Signature**: `isPublicCapability(address,bytes4)`
- **Visibility**: public
- **Source Range**: 2009:175:37
- **Details**: [function_isPublicCapability_address_bytes4.md](./function_isPublicCapability_address_bytes4.md)

**Signature:**
```solidity
function isPublicCapability(address target, bytes4 functionSig) public view returns (bool);
```

### canCall(address,address,bytes4) (inherited from RolesAuthority)

- **Signature**: `canCall(address,address,bytes4)`
- **Visibility**: public
- **Source Range**: 2652:490:37
- **Details**: [function_canCall_address_address_bytes4.md](./function_canCall_address_address_bytes4.md)

**Signature:**
```solidity
/// @notice A user can call a given function signature on a given target address if:
/// - The capability has not been burned
/// - That capability is public, or the user has a role that has been granted the capability to call the function
function canCall(address user, address target, bytes4 functionSig) virtual override public view returns (bool);
```

### setPublicCapability(address,bytes4,bool) (inherited from RolesAuthority)

- **Signature**: `setPublicCapability(address,bytes4,bool)`
- **Visibility**: public
- **Source Range**: 3522:558:37
- **Details**: [function_setPublicCapability_address_bytes4_bool.md](./function_setPublicCapability_address_bytes4_bool.md)

**Signature:**
```solidity
/// @notice Set a capability flag as public, meaning any account can call it. Or revoke this capability.
///  @dev A capability cannot be made public if it has been burned.
function setPublicCapability(address target, bytes4 functionSig, bool enabled) virtual public requiresAuth();
```

### setRoleCapability(uint8,address,bytes4,bool) (inherited from RolesAuthority)

- **Signature**: `setRoleCapability(uint8,address,bytes4,bool)`
- **Visibility**: public
- **Source Range**: 4199:1083:37
- **Details**: [function_setRoleCapability_uint8_address_bytes4_bool.md](./function_setRoleCapability_uint8_address_bytes4_bool.md)

**Signature:**
```solidity
/// @notice Grant a specified role the ability to call a function on a target.
///  @notice Has no effect
function setRoleCapability(uint8 role, address target, bytes4 functionSig, bool enabled) virtual public requiresAuth();
```

### burnCapability(address,bytes4) (inherited from RolesAuthority)

- **Signature**: `burnCapability(address,bytes4)`
- **Visibility**: public
- **Source Range**: 5349:367:37
- **Details**: [function_burnCapability_address_bytes4.md](./function_burnCapability_address_bytes4.md)

**Signature:**
```solidity
/// @notice Permanently burns a capability for a target.
function burnCapability(address target, bytes4 functionSig) virtual public requiresAuth();
```

### setUserRole(address,uint8,bool) (inherited from RolesAuthority)

- **Signature**: `setUserRole(address,uint8,bool)`
- **Visibility**: public
- **Source Range**: 5911:543:37
- **Details**: [function_setUserRole_address_uint8_bool.md](./function_setUserRole_address_uint8_bool.md)

**Signature:**
```solidity
function setUserRole(address user, uint8 role, bool enabled) virtual public requiresAuth();
```
