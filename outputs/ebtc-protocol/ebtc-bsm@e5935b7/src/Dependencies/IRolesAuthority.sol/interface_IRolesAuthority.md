# Interface: IRolesAuthority

## Metadata

- **Name**: IRolesAuthority
- **Type**: Interface
- **Path**: src/Dependencies/IRolesAuthority.sol
- **Documentation**: @notice Role based Authority that supports up to 256 roles.
   @author BadgerDAO
   @author Modified from Solmate (https://github.com/transmissions11/solmate/blob/main/src/auth/authorities/RolesAuthority.sol)
   @author Modified from Dappsys (https://github.com/dapphub/ds-roles/blob/master/src/roles.sol)

## Events

### UserRoleUpdated

```solidity
event UserRoleUpdated(address indexed user, uint8 indexed role, bool enabled);
```

### PublicCapabilityUpdated

```solidity
event PublicCapabilityUpdated(address indexed target, bytes4 indexed functionSig, bool enabled);
```

### CapabilityBurned

```solidity
event CapabilityBurned(address indexed target, bytes4 indexed functionSig);
```

### RoleCapabilityUpdated

```solidity
event RoleCapabilityUpdated(uint8 indexed role, address indexed target, bytes4 indexed functionSig, bool enabled);
```

## Enums

### CapabilityFlag

```solidity
enum CapabilityFlag {
    None,
    Public,
    Burned
}
```
