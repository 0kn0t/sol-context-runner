# Function: operationBatch(bytes32)

**Contract**: [lib/openzeppelin-community-contracts/contracts/governance/TimelockControllerEnumerable.sol/contract_TimelockControllerEnumerable.md]

## Metadata

- **Contract**: TimelockControllerEnumerable
- **Signature**: `operationBatch(bytes32)`
- **Visibility**: public
- **Source Range**: 6986:296:53

## Implementation

```solidity
/// @dev Return the operationsBatch with the given id
///  @param id The id of the operationsBatch
///  @return operationBatch_ The operationsBatch
function operationBatch(bytes32 id) public view returns (OperationBatch memory operationBatch_) {
    if (!_operationsBatchIdSet.contains(id)) {
        revert OperationBatchIdNotFound(id);
    }
    operationBatch_ = _operationsBatchMap[id];
    return operationBatch_;
}
```

## Related Implementations

### contains(struct EnumerableSet.Bytes32Set,bytes32)

- **Kind**: internal
- **Source**: 7474:138:112
- **Link**: `lib/openzeppelin-contracts/contracts/utils/structs/EnumerableSet.sol:EnumerableSet:contains(struct EnumerableSet.Bytes32Set,bytes32)`

```solidity
///  @dev Returns true if the value is in the set. O(1).
function contains(Bytes32Set storage set, bytes32 value) internal view returns (bool) {
    return _contains(set._inner, value);
}
```

### _contains(struct EnumerableSet.Set,bytes32)

- **Kind**: internal
- **Source**: 4910:129:112
- **Link**: `lib/openzeppelin-contracts/contracts/utils/structs/EnumerableSet.sol:EnumerableSet:_contains(struct EnumerableSet.Set,bytes32)`

```solidity
///  @dev Returns true if the value is in the set. O(1).
function _contains(Set storage set, bytes32 value) private view returns (bool) {
    return set._positions[value] != 0;
}
```

## State Variable Reads

- **_operationsBatchIdSet** (`struct EnumerableSet.Bytes32Set`)
- **_operationsBatchMap** (`mapping(bytes32 => struct TimelockControllerEnumerable.OperationBatch)`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: TimelockControllerEnumerable.operationBatch(bytes32) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
  └─ [1] ⚙️ FUNCTION: EnumerableSet.contains(struct EnumerableSet.Bytes32Set,bytes32) (NodeID: 1)
      💬 Args: [_operationsBatchIdSet, id]
      👁️  Def: internal
    └─ [2] ⚙️ FUNCTION: EnumerableSet._contains(struct EnumerableSet.Set,bytes32) (NodeID: 2)
        💬 Args: [set._inner, value]
        👁️  Def: private
```

## Documentation

### Function Documentation

@dev Return the operationsBatch with the given id
 @param id The id of the operationsBatch
 @return operationBatch_ The operationsBatch
