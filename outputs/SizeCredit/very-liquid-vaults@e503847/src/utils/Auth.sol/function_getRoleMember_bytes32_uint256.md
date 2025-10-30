# Function: getRoleMember(bytes32,uint256)

**Contract**: [src/utils/Auth.sol/contract_Auth.md]

## Metadata

- **Contract**: Auth
- **Signature**: `getRoleMember(bytes32,uint256)`
- **Visibility**: public
- **Source Range**: 2492:233:12
- **Inherited From**: AccessControlEnumerableUpgradeable

## Implementation

```solidity
///  @dev Returns one of the accounts that have `role`. `index` must be a
///  value between 0 and {getRoleMemberCount}, non-inclusive.
///  Role bearers are not sorted in any particular way, and their ordering may
///  change at any point.
///  WARNING: When using {getRoleMember} and {getRoleMemberCount}, make sure
///  you perform all queries on the same block. See the following
///  https://forum.openzeppelin.com/t/iterating-over-elements-on-enumerableset-in-openzeppelin-contracts/2296[forum post]
///  for more information.
function getRoleMember(bytes32 role, uint256 index) virtual public view returns (address) {
    AccessControlEnumerableStorage storage $ = _getAccessControlEnumerableStorage();
    return $._roleMembers[role].at(index);
}
```

## Related Implementations

### _getAccessControlEnumerableStorage()

- **Kind**: internal
- **Source**: 1250:207:12
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/access/extensions/AccessControlEnumerableUpgradeable.sol:AccessControlEnumerableUpgradeable:_getAccessControlEnumerableStorage()`

```solidity
function _getAccessControlEnumerableStorage() private pure returns (AccessControlEnumerableStorage storage $) {
    assembly {
        $.slot := AccessControlEnumerableStorageLocation
    }
}
```

### at(struct EnumerableSet.AddressSet,uint256)

- **Kind**: internal
- **Source**: 10987:156:55
- **Link**: `lib/openzeppelin-contracts/contracts/utils/structs/EnumerableSet.sol:EnumerableSet:at(struct EnumerableSet.AddressSet,uint256)`

```solidity
///  @dev Returns the value stored at position `index` in the set. O(1).
///  Note that there are no guarantees on the ordering of values inside the
///  array, and it may change when more values are added or removed.
///  Requirements:
///  - `index` must be strictly less than {length}.
function at(AddressSet storage set, uint256 index) internal view returns (address) {
    return address(uint160(uint256(_at(set._inner, index))));
}
```

### _at(struct EnumerableSet.Set,uint256)

- **Kind**: internal
- **Source**: 5569:118:55
- **Link**: `lib/openzeppelin-contracts/contracts/utils/structs/EnumerableSet.sol:EnumerableSet:_at(struct EnumerableSet.Set,uint256)`

```solidity
///  @dev Returns the value stored at position `index` in the set. O(1).
///  Note that there are no guarantees on the ordering of values inside the
///  array, and it may change when more values are added or removed.
///  Requirements:
///  - `index` must be strictly less than {length}.
function _at(Set storage set, uint256 index) private view returns (bytes32) {
    return set._values[index];
}
```

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: AccessControlEnumerableUpgradeable.getRoleMember(bytes32,uint256) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
  ├─ [1] ⚙️ FUNCTION: AccessControlEnumerableUpgradeable._getAccessControlEnumerableStorage() (NodeID: 1)
  │   💬 Args: [no args]
  │   👁️  Def: private
  └─ [1] ⚙️ FUNCTION: EnumerableSet.at(struct EnumerableSet.AddressSet,uint256) (NodeID: 2)
      💬 Args: [$._roleMembers[role], index]
      👁️  Def: internal
    └─ [2] ⚙️ FUNCTION: EnumerableSet._at(struct EnumerableSet.Set,uint256) (NodeID: 3)
        💬 Args: [set._inner, index]
        👁️  Def: private
```

## Documentation

### Function Documentation

 @dev Returns one of the accounts that have `role`. `index` must be a
 value between 0 and {getRoleMemberCount}, non-inclusive.
 Role bearers are not sorted in any particular way, and their ordering may
 change at any point.
 WARNING: When using {getRoleMember} and {getRoleMemberCount}, make sure
 you perform all queries on the same block. See the following
 https://forum.openzeppelin.com/t/iterating-over-elements-on-enumerableset-in-openzeppelin-contracts/2296[forum post]
 for more information.

### Interface Documentation

 @dev Returns one of the accounts that have `role`. `index` must be a
 value between 0 and {getRoleMemberCount}, non-inclusive.
 Role bearers are not sorted in any particular way, and their ordering may
 change at any point.
 WARNING: When using {getRoleMember} and {getRoleMemberCount}, make sure
 you perform all queries on the same block. See the following
 https://forum.openzeppelin.com/t/iterating-over-elements-on-enumerableset-in-openzeppelin-contracts/2296[forum post]
 for more information.
