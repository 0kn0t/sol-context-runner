# Function: getUsersByRole(uint8)

**Contract**: [src/Dependencies/Governor.sol/contract_Governor.md]

## Metadata

- **Contract**: Governor
- **Signature**: `getUsersByRole(uint8)`
- **Visibility**: external
- **Source Range**: 1811:856:28

## Implementation

```solidity
/// @notice Returns a list of users that are assigned a specific role.
///  @dev This function searches all users and checks if they are assigned the given role.
///  @dev Intended for off-chain utility only due to inefficiency.
///  @param role The role ID to find users for.
///  @return usersWithRole An array of addresses that are assigned the given role.
function getUsersByRole(uint8 role) external view returns (address[] memory usersWithRole) {
    uint256 count;
    for (uint256 i = 0; i < users.length(); i++) {
        address user = users.at(i);
        bool _canCall = doesUserHaveRole(user, role);
        if (_canCall) {
            count += 1;
        }
    }
    if (count > 0) {
        uint256 j = 0;
        usersWithRole = new address[](count);
        address[] memory _usrs = users.values();
        for (uint256 i = 0; i < _usrs.length; i++) {
            address user = _usrs[i];
            bool _canCall = doesUserHaveRole(user, role);
            if (_canCall) {
                usersWithRole[j] = user;
                j++;
            }
        }
    }
}
```

## Related Implementations

### length(struct EnumerableSet.AddressSet)

- **Kind**: internal
- **Source**: 9106:115:27
- **Link**: `src/Dependencies/EnumerableSet.sol:EnumerableSet:length(struct EnumerableSet.AddressSet)`

```solidity
///  @dev Returns the number of values in the set. O(1).
function length(AddressSet storage set) internal view returns (uint256) {
    return _length(set._inner);
}
```

### _length(struct EnumerableSet.Set)

- **Kind**: internal
- **Source**: 4463:107:27
- **Link**: `src/Dependencies/EnumerableSet.sol:EnumerableSet:_length(struct EnumerableSet.Set)`

```solidity
///  @dev Returns the number of values on the set. O(1).
function _length(Set storage set) private view returns (uint256) {
    return set._values.length;
}
```

### at(struct EnumerableSet.AddressSet,uint256)

- **Kind**: internal
- **Source**: 9563:156:27
- **Link**: `src/Dependencies/EnumerableSet.sol:EnumerableSet:at(struct EnumerableSet.AddressSet,uint256)`

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
- **Source**: 4912:118:27
- **Link**: `src/Dependencies/EnumerableSet.sol:EnumerableSet:_at(struct EnumerableSet.Set,uint256)`

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

### doesUserHaveRole(address,uint8)

- **Kind**: internal
- **Source**: 1600:157:37
- **Link**: `src/Dependencies/RolesAuthority.sol:RolesAuthority:doesUserHaveRole(address,uint8)`

```solidity
function doesUserHaveRole(address user, uint8 role) virtual public view returns (bool) {
    return ((uint256(getUserRoles[user]) >> role) & 1) != 0;
}
```

### values(struct EnumerableSet.AddressSet)

- **Kind**: internal
- **Source**: 10259:300:27
- **Link**: `src/Dependencies/EnumerableSet.sol:EnumerableSet:values(struct EnumerableSet.AddressSet)`

```solidity
///  @dev Return the entire set in an array
///  WARNING: This operation will copy the entire storage to memory, which can be quite expensive. This is designed
///  to mostly be used by view accessors that are queried without any gas fees. Developers should keep in mind that
///  this function has an unbounded cost, and using it as part of a state-changing function may render the function
///  uncallable if the set grows to a point where copying to memory consumes too much gas to fit in a block.
function values(AddressSet storage set) internal view returns (address[] memory) {
    bytes32[] memory store = _values(set._inner);
    address[] memory result;
    /// @solidity memory-safe-assembly
    assembly {
        result := store
    }
    return result;
}
```

### _values(struct EnumerableSet.Set)

- **Kind**: internal
- **Source**: 5570:109:27
- **Link**: `src/Dependencies/EnumerableSet.sol:EnumerableSet:_values(struct EnumerableSet.Set)`

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

## State Variable Reads

- **getUserRoles** (`mapping(address => bytes32)`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: Governor.getUsersByRole(uint8) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: external
  ├─ [1] ⚙️ FUNCTION: EnumerableSet.length(struct EnumerableSet.AddressSet) (NodeID: 1)
  │   💬 Args: [users]
  │   👁️  Def: internal
  │ └─ [2] ⚙️ FUNCTION: EnumerableSet._length(struct EnumerableSet.Set) (NodeID: 2)
  │     💬 Args: [set._inner]
  │     👁️  Def: private
  ├─ [1] ⚙️ FUNCTION: EnumerableSet.at(struct EnumerableSet.AddressSet,uint256) (NodeID: 3)
  │   💬 Args: [users, i]
  │   👁️  Def: internal
  │ └─ [2] ⚙️ FUNCTION: EnumerableSet._at(struct EnumerableSet.Set,uint256) (NodeID: 4)
  │     💬 Args: [set._inner, index]
  │     👁️  Def: private
  ├─ [1] ⚙️ FUNCTION: RolesAuthority.doesUserHaveRole(address,uint8) (NodeID: 5)
  │   💬 Args: [user, role]
  │   👁️  Def: public
  ├─ [1] ⚙️ FUNCTION: EnumerableSet.values(struct EnumerableSet.AddressSet) (NodeID: 6)
  │   💬 Args: [users]
  │   👁️  Def: internal
  │ └─ [2] ⚙️ FUNCTION: EnumerableSet._values(struct EnumerableSet.Set) (NodeID: 7)
  │     💬 Args: [set._inner]
  │     👁️  Def: private
  └─ [1] ⚙️ FUNCTION: RolesAuthority.doesUserHaveRole(address,uint8) (NodeID: 8)
      💬 Args: [user, role]
      👁️  Def: public
```

## Documentation

### Function Documentation

@notice Returns a list of users that are assigned a specific role.
 @dev This function searches all users and checks if they are assigned the given role.
 @dev Intended for off-chain utility only due to inefficiency.
 @param role The role ID to find users for.
 @return usersWithRole An array of addresses that are assigned the given role.
