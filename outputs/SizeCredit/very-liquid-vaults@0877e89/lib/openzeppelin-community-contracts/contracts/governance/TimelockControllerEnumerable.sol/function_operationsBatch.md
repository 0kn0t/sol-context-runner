# Function: operationsBatch()

**Contract**: [lib/openzeppelin-community-contracts/contracts/governance/TimelockControllerEnumerable.sol/contract_TimelockControllerEnumerable.md]

## Metadata

- **Contract**: TimelockControllerEnumerable
- **Signature**: `operationsBatch()`
- **Visibility**: public
- **Source Range**: 5553:430:53

## Implementation

```solidity
/// @dev Return the operationsBatch
///  @return operationsBatch_ The operationsBatch array
function operationsBatch() public view returns (OperationBatch[] memory operationsBatch_) {
    uint256 operationsBatchCount_ = _operationsBatchIdSet.length();
    operationsBatch_ = new OperationBatch[](operationsBatchCount_);
    for (uint256 i = 0; i < operationsBatchCount_; i++) {
        operationsBatch_[i] = _operationsBatchMap[_operationsBatchIdSet.at(i)];
    }
    return operationsBatch_;
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
┌─ [0] ⚙️ FUNCTION: TimelockControllerEnumerable.operationsBatch() (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
  ├─ [1] ⚙️ FUNCTION: EnumerableSet.length(struct EnumerableSet.Bytes32Set) (NodeID: 1)
  │   💬 Args: [_operationsBatchIdSet]
  │   👁️  Def: internal
  │ └─ [2] ⚙️ FUNCTION: EnumerableSet._length(struct EnumerableSet.Set) (NodeID: 2)
  │     💬 Args: [set._inner]
  │     👁️  Def: private
  └─ [1] ⚙️ FUNCTION: EnumerableSet.at(struct EnumerableSet.Bytes32Set,uint256) (NodeID: 3)
      💬 Args: [_operationsBatchIdSet, i]
      👁️  Def: internal
    └─ [2] ⚙️ FUNCTION: EnumerableSet._at(struct EnumerableSet.Set,uint256) (NodeID: 4)
        💬 Args: [set._inner, index]
        👁️  Def: private
```

## Documentation

### Function Documentation

@dev Return the operationsBatch
 @return operationsBatch_ The operationsBatch array
