# Function: getRoleMembers(bytes32)

**Contract**: [src/utils/Auth.sol/contract_Auth.md]

## Metadata

- **Contract**: Auth
- **Signature**: `getRoleMembers(bytes32)`
- **Visibility**: public
- **Source Range**: 3658:227:12
- **Inherited From**: AccessControlEnumerableUpgradeable

## Implementation

```solidity
///  @dev Return all accounts that have `role`
///  WARNING: This operation will copy the entire storage to memory, which can be quite expensive. This is designed
///  to mostly be used by view accessors that are queried without any gas fees. Developers should keep in mind that
///  this function has an unbounded cost, and using it as part of a state-changing function may render the function
///  uncallable if the set grows to a point where copying to memory consumes too much gas to fit in a block.
function getRoleMembers(bytes32 role) virtual public view returns (address[] memory) {
    AccessControlEnumerableStorage storage $ = _getAccessControlEnumerableStorage();
    return $._roleMembers[role].values();
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

### values(struct EnumerableSet.AddressSet)

- **Kind**: internal
- **Source**: 11683:273:55
- **Link**: `lib/openzeppelin-contracts/contracts/utils/structs/EnumerableSet.sol:EnumerableSet:values(struct EnumerableSet.AddressSet)`

```solidity
///  @dev Return the entire set in an array
///  WARNING: This operation will copy the entire storage to memory, which can be quite expensive. This is designed
///  to mostly be used by view accessors that are queried without any gas fees. Developers should keep in mind that
///  this function has an unbounded cost, and using it as part of a state-changing function may render the function
///  uncallable if the set grows to a point where copying to memory consumes too much gas to fit in a block.
function values(AddressSet storage set) internal view returns (address[] memory) {
    bytes32[] memory store = _values(set._inner);
    address[] memory result;
    assembly ("memory-safe") {
        result := store
    }
    return result;
}
```

### _values(struct EnumerableSet.Set)

- **Kind**: internal
- **Source**: 6227:109:55
- **Link**: `lib/openzeppelin-contracts/contracts/utils/structs/EnumerableSet.sol:EnumerableSet:_values(struct EnumerableSet.Set)`

```solidity
///  @dev Return the entire set in an array
///  WARNING: This operation will copy the entire storage to memory, which can be quite expensive. This is designed
///  to mostly be used by view accessors that are queried without any gas fees. Developers should keep in mind that
///  this function has an unbounded cost, and using it as part of a state-changing function may render the function
///  uncallable if the set grows to a point where copying to memory consumes too much gas to fit in a block.
function _values(Set storage set) private view returns (bytes32[] memory) {
    return set._values;
}
```

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: AccessControlEnumerableUpgradeable.getRoleMembers(bytes32) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
  ├─ [1] ⚙️ FUNCTION: AccessControlEnumerableUpgradeable._getAccessControlEnumerableStorage() (NodeID: 1)
  │   💬 Args: [no args]
  │   👁️  Def: private
  └─ [1] ⚙️ FUNCTION: EnumerableSet.values(struct EnumerableSet.AddressSet) (NodeID: 2)
      💬 Args: [$._roleMembers[role]]
      👁️  Def: internal
    └─ [2] ⚙️ FUNCTION: EnumerableSet._values(struct EnumerableSet.Set) (NodeID: 3)
        💬 Args: [set._inner]
        👁️  Def: private
```

## Documentation

### Function Documentation

 @dev Return all accounts that have `role`
 WARNING: This operation will copy the entire storage to memory, which can be quite expensive. This is designed
 to mostly be used by view accessors that are queried without any gas fees. Developers should keep in mind that
 this function has an unbounded cost, and using it as part of a state-changing function may render the function
 uncallable if the set grows to a point where copying to memory consumes too much gas to fit in a block.
