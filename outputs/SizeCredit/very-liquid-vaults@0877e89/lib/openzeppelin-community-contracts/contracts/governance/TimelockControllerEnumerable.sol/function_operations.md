# Function: operations()

**Contract**: [lib/openzeppelin-community-contracts/contracts/governance/TimelockControllerEnumerable.sol/contract_TimelockControllerEnumerable.md]

## Metadata

- **Contract**: TimelockControllerEnumerable
- **Signature**: `operations()`
- **Visibility**: public
- **Source Range**: 3955:365:53

## Implementation

```solidity
/// @dev Return the operations
///  @return operations_ The operations array
function operations() public view returns (Operation[] memory operations_) {
    uint256 operationsCount_ = _operationsIdSet.length();
    operations_ = new Operation[](operationsCount_);
    for (uint256 i = 0; i < operationsCount_; i++) {
        operations_[i] = _operationsMap[_operationsIdSet.at(i)];
    }
    return operations_;
}
```

## Related Implementations

### length(struct EnumerableSet.Bytes32Set)

- **Kind**: internal
- **Source**: 7693:115:112
- **Link**: `lib/openzeppelin-contracts/contracts/utils/structs/EnumerableSet.sol:EnumerableSet:length(struct EnumerableSet.Bytes32Set)`

```solidity
///  @dev Returns the number of values in the set. O(1).
function length(Bytes32Set storage set) internal view returns (uint256) {
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

### at(struct EnumerableSet.Bytes32Set,uint256)

- **Kind**: internal
- **Source**: 8150:129:112
- **Link**: `lib/openzeppelin-contracts/contracts/utils/structs/EnumerableSet.sol:EnumerableSet:at(struct EnumerableSet.Bytes32Set,uint256)`

```solidity
///  @dev Returns the value stored at position `index` in the set. O(1).
///  Note that there are no guarantees on the ordering of values inside the
///  array, and it may change when more values are added or removed.
///  Requirements:
///  - `index` must be strictly less than {length}.
function at(Bytes32Set storage set, uint256 index) internal view returns (bytes32) {
    return _at(set._inner, index);
}
```

### _at(struct EnumerableSet.Set,uint256)

- **Kind**: internal
- **Source**: 5569:118:112
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

## State Variable Reads

- **_operationsIdSet** (`struct EnumerableSet.Bytes32Set`)
- **_operationsMap** (`mapping(bytes32 => struct TimelockControllerEnumerable.Operation)`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: TimelockControllerEnumerable.operations() (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
  ├─ [1] ⚙️ FUNCTION: EnumerableSet.length(struct EnumerableSet.Bytes32Set) (NodeID: 1)
  │   💬 Args: [_operationsIdSet]
  │   👁️  Def: internal
  │ └─ [2] ⚙️ FUNCTION: EnumerableSet._length(struct EnumerableSet.Set) (NodeID: 2)
  │     💬 Args: [set._inner]
  │     👁️  Def: private
  └─ [1] ⚙️ FUNCTION: EnumerableSet.at(struct EnumerableSet.Bytes32Set,uint256) (NodeID: 3)
      💬 Args: [_operationsIdSet, i]
      👁️  Def: internal
    └─ [2] ⚙️ FUNCTION: EnumerableSet._at(struct EnumerableSet.Set,uint256) (NodeID: 4)
        💬 Args: [set._inner, index]
        👁️  Def: private
```

## Documentation

### Function Documentation

@dev Return the operations
 @return operations_ The operations array
