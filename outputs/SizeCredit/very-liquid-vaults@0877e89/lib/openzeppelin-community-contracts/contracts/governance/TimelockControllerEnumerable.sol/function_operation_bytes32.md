# Function: operation(bytes32)

**Contract**: [lib/openzeppelin-community-contracts/contracts/governance/TimelockControllerEnumerable.sol/contract_TimelockControllerEnumerable.md]

## Metadata

- **Contract**: TimelockControllerEnumerable
- **Signature**: `operation(bytes32)`
- **Visibility**: public
- **Source Range**: 5192:256:53

## Implementation

```solidity
/// @dev Return the operation with the given id
///  @param id The id of the operation
///  @return operation_ The operation
function operation(bytes32 id) public view returns (Operation memory operation_) {
    if (!_operationsIdSet.contains(id)) {
        revert OperationIdNotFound(id);
    }
    operation_ = _operationsMap[id];
    return operation_;
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

- **_operationsIdSet** (`struct EnumerableSet.Bytes32Set`)
- **_operationsMap** (`mapping(bytes32 => struct TimelockControllerEnumerable.Operation)`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: TimelockControllerEnumerable.operation(bytes32) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
  └─ [1] ⚙️ FUNCTION: EnumerableSet.contains(struct EnumerableSet.Bytes32Set,bytes32) (NodeID: 1)
      💬 Args: [_operationsIdSet, id]
      👁️  Def: internal
    └─ [2] ⚙️ FUNCTION: EnumerableSet._contains(struct EnumerableSet.Set,bytes32) (NodeID: 2)
        💬 Args: [set._inner, value]
        👁️  Def: private
```

## Documentation

### Function Documentation

@dev Return the operation with the given id
 @param id The id of the operation
 @return operation_ The operation
