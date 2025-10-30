# Contract: AuthNoOwner

## Metadata

- **Name**: AuthNoOwner
- **Type**: Contract
- **Path**: src/Dependencies/AuthNoOwner.sol
- **Documentation**: @notice Provides a flexible and updatable auth pattern which is completely separate from application logic.
   @author Modified by BadgerDAO to remove owner
   @author Solmate (https://github.com/transmissions11/solmate/blob/main/src/auth/Auth.sol)
   @author Modified from Dappsys (https://github.com/dapphub/ds-auth/blob/master/src/auth.sol)

## State Variables

### _authority

```solidity
Authority private _authority
```

**Authority**: [src/Dependencies/Authority.sol/interface_Authority.md]

### _authorityInitialized

```solidity
bool private _authorityInitialized
```

## Events

### AuthorityUpdated

```solidity
event AuthorityUpdated(address indexed user, Authority indexed newAuthority);
```

## Public/External Functions

### authority()

- **Signature**: `authority()`
- **Visibility**: public
- **Source Range**: 778:87:25
- **Details**: [function_authority.md](./function_authority.md)

**Signature:**
```solidity
function authority() public view returns (Authority);
```

### authorityInitialized()

- **Signature**: `authorityInitialized()`
- **Visibility**: public
- **Source Range**: 871:104:25
- **Details**: [function_authorityInitialized.md](./function_authorityInitialized.md)

**Signature:**
```solidity
function authorityInitialized() public view returns (bool);
```
