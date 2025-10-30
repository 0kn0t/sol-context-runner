# Function: getRoleMemberCount(bytes32)

**Contract**: [src/Auth.sol/contract_Auth.md]

## Metadata

- **Contract**: Auth
- **Signature**: `getRoleMemberCount(bytes32)`
- **Visibility**: public
- **Source Range**: 2893:222:55
- **Inherited From**: AccessControlEnumerableUpgradeable

## Implementation

```solidity
///  @dev Returns the number of accounts that have `role`. Can be used
///  together with {getRoleMember} to enumerate all bearers of a role.
function getRoleMemberCount(bytes32 role) virtual public view returns (uint256) {
    AccessControlEnumerableStorage storage $ = _getAccessControlEnumerableStorage();
    return $._roleMembers[role].length();
}
```

## Related Implementations

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

### length(struct EnumerableSet.AddressSet)

- **Kind**: internal
- **Source**: 10530:115:112
- **Link**: `lib/openzeppelin-contracts/contracts/utils/structs/EnumerableSet.sol:EnumerableSet:length(struct EnumerableSet.AddressSet)`

```solidity
///  @dev Returns the number of values in the set. O(1).
function length(AddressSet storage set) internal view returns (uint256) {
    return _length(set._inner);
}
```

### _length(struct EnumerableSet.Set)

- **Kind**: internal
- **Source**: 5120:107:112
- **Link**: `lib/openzeppelin-contracts/contracts/utils/structs/EnumerableSet.sol:EnumerableSet:_length(struct EnumerableSet.Set)`

```solidity
///  @dev Returns the number of values on the set. O(1).
function _length(Set storage set) private view returns (uint256) {
    return set._values.length;
}
```

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: AccessControlEnumerableUpgradeable.getRoleMemberCount(bytes32) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
  ├─ [1] ⚙️ FUNCTION: AccessControlEnumerableUpgradeable._getAccessControlEnumerableStorage() (NodeID: 1)
  │   💬 Args: [no args]
  │   👁️  Def: private
  └─ [1] ⚙️ FUNCTION: EnumerableSet.length(struct EnumerableSet.AddressSet) (NodeID: 2)
      💬 Args: [$._roleMembers[role]]
      👁️  Def: internal
    └─ [2] ⚙️ FUNCTION: EnumerableSet._length(struct EnumerableSet.Set) (NodeID: 3)
        💬 Args: [set._inner]
        👁️  Def: private
```

## Documentation

### Function Documentation

 @dev Returns the number of accounts that have `role`. Can be used
 together with {getRoleMember} to enumerate all bearers of a role.

### Interface Documentation

 @dev Returns the number of accounts that have `role`. Can be used
 together with {getRoleMember} to enumerate all bearers of a role.
