# Function: hasRole(bytes32,address)

**Contract**: [src/Auth.sol/contract_Auth.md]

## Metadata

- **Contract**: Auth
- **Signature**: `hasRole(bytes32,address)`
- **Visibility**: public
- **Source Range**: 3732:207:54
- **Inherited From**: AccessControlUpgradeable

## Implementation

```solidity
///  @dev Returns `true` if `account` has been granted `role`.
function hasRole(bytes32 role, address account) virtual public view returns (bool) {
    AccessControlStorage storage $ = _getAccessControlStorage();
    return $._roles[role].hasRole[account];
}
```

## Related Implementations

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

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: AccessControlUpgradeable.hasRole(bytes32,address) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
  └─ [1] ⚙️ FUNCTION: AccessControlUpgradeable._getAccessControlStorage() (NodeID: 1)
      💬 Args: [no args]
      👁️  Def: private
```

## Documentation

### Function Documentation

 @dev Returns `true` if `account` has been granted `role`.

### Interface Documentation

 @dev Returns `true` if `account` has been granted `role`.
