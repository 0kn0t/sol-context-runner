# Contract: RolesAuthority

## Metadata

- **Name**: RolesAuthority
- **Type**: Contract
- **Path**: src/Dependencies/RolesAuthority.sol
- **Documentation**: @notice Role based Authority that supports up to 256 roles.
   @author BadgerDAO
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

### users

```solidity
EnumerableSet.AddressSet internal users
```

### targets

```solidity
EnumerableSet.AddressSet internal targets
```

### enabledFunctionSigsByTarget

```solidity
mapping(address => EnumerableSet.Bytes32Set) internal enabledFunctionSigsByTarget
```

### enabledFunctionSigsPublic

```solidity
EnumerableSet.Bytes32Set internal enabledFunctionSigsPublic
```

### getUserRoles

```solidity
mapping(address => bytes32) public getUserRoles
```

### capabilityFlag

```solidity
mapping(address => mapping(bytes4 => CapabilityFlag)) public capabilityFlag
```

### getRolesWithCapability

```solidity
mapping(address => mapping(bytes4 => bytes32)) public getRolesWithCapability
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

### constructor(address,contract Authority)

- **Signature**: `constructor(address,contract Authority)`
- **Visibility**: public
- **Source Range**: 867:77:37
- **Details**: [function_constructor_address_contract_Authority.md](./function_constructor_address_contract_Authority.md)

**Signature:**
```solidity
constructor(address _owner, Authority _authority) Auth(_owner,_authority);
```

### doesUserHaveRole(address,uint8)

- **Signature**: `doesUserHaveRole(address,uint8)`
- **Visibility**: public
- **Source Range**: 1600:157:37
- **Details**: [function_doesUserHaveRole_address_uint8.md](./function_doesUserHaveRole_address_uint8.md)

**Signature:**
```solidity
function doesUserHaveRole(address user, uint8 role) virtual public view returns (bool);
```

### doesRoleHaveCapability(uint8,address,bytes4)

- **Signature**: `doesRoleHaveCapability(uint8,address,bytes4)`
- **Visibility**: public
- **Source Range**: 1763:240:37
- **Details**: [function_doesRoleHaveCapability_uint8_address_bytes4.md](./function_doesRoleHaveCapability_uint8_address_bytes4.md)

**Signature:**
```solidity
function doesRoleHaveCapability(uint8 role, address target, bytes4 functionSig) virtual public view returns (bool);
```

### isPublicCapability(address,bytes4)

- **Signature**: `isPublicCapability(address,bytes4)`
- **Visibility**: public
- **Source Range**: 2009:175:37
- **Details**: [function_isPublicCapability_address_bytes4.md](./function_isPublicCapability_address_bytes4.md)

**Signature:**
```solidity
function isPublicCapability(address target, bytes4 functionSig) public view returns (bool);
```

### canCall(address,address,bytes4)

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

### setPublicCapability(address,bytes4,bool)

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

### setRoleCapability(uint8,address,bytes4,bool)

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

### burnCapability(address,bytes4)

- **Signature**: `burnCapability(address,bytes4)`
- **Visibility**: public
- **Source Range**: 5349:367:37
- **Details**: [function_burnCapability_address_bytes4.md](./function_burnCapability_address_bytes4.md)

**Signature:**
```solidity
/// @notice Permanently burns a capability for a target.
function burnCapability(address target, bytes4 functionSig) virtual public requiresAuth();
```

### setUserRole(address,uint8,bool)

- **Signature**: `setUserRole(address,uint8,bool)`
- **Visibility**: public
- **Source Range**: 5911:543:37
- **Details**: [function_setUserRole_address_uint8_bool.md](./function_setUserRole_address_uint8_bool.md)

**Signature:**
```solidity
function setUserRole(address user, uint8 role, bool enabled) virtual public requiresAuth();
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
