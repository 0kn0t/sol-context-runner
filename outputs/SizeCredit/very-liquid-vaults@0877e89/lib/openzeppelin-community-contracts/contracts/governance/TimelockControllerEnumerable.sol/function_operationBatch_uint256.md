# Function: operationBatch(uint256)

**Contract**: [lib/openzeppelin-community-contracts/contracts/governance/TimelockControllerEnumerable.sol/contract_TimelockControllerEnumerable.md]

## Metadata

- **Contract**: TimelockControllerEnumerable
- **Signature**: `operationBatch(uint256)`
- **Visibility**: public
- **Source Range**: 6484:338:53

## Implementation

```solidity
/// @dev Return the operationsBatch at the given index
///  @param index The index of the operationsBatch
///  @return operationBatch_ The operationsBatch
function operationBatch(uint256 index) public view returns (OperationBatch memory operationBatch_) {
    if (index >= _operationsBatchIdSet.length()) {
        revert OperationBatchIndexNotFound(index);
    }
    operationBatch_ = _operationsBatchMap[_operationsBatchIdSet.at(index)];
    return operationBatch_;
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

- **_operationsBatchIdSet** (`struct EnumerableSet.Bytes32Set`)
- **_operationsBatchMap** (`mapping(bytes32 => struct TimelockControllerEnumerable.OperationBatch)`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: TimelockControllerEnumerable.operationBatch(uint256) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
  ├─ [1] ⚙️ FUNCTION: EnumerableSet.length(struct EnumerableSet.Bytes32Set) (NodeID: 1)
  │   💬 Args: [_operationsBatchIdSet]
  │   👁️  Def: internal
  │ └─ [2] ⚙️ FUNCTION: EnumerableSet._length(struct EnumerableSet.Set) (NodeID: 2)
  │     💬 Args: [set._inner]
  │     👁️  Def: private
  └─ [1] ⚙️ FUNCTION: EnumerableSet.at(struct EnumerableSet.Bytes32Set,uint256) (NodeID: 3)
      💬 Args: [_operationsBatchIdSet, index]
      👁️  Def: internal
    └─ [2] ⚙️ FUNCTION: EnumerableSet._at(struct EnumerableSet.Set,uint256) (NodeID: 4)
        💬 Args: [set._inner, index]
        👁️  Def: private
```

## Documentation

### Function Documentation

@dev Return the operationsBatch at the given index
 @param index The index of the operationsBatch
 @return operationBatch_ The operationsBatch
