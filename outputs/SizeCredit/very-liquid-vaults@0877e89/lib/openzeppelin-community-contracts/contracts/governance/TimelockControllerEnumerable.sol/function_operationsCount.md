# Function: operationsCount()

**Contract**: [lib/openzeppelin-community-contracts/contracts/governance/TimelockControllerEnumerable.sol/contract_TimelockControllerEnumerable.md]

## Metadata

- **Contract**: TimelockControllerEnumerable
- **Signature**: `operationsCount()`
- **Visibility**: public
- **Source Range**: 4442:168:53

## Implementation

```solidity
/// @dev Return the number of operations from the set
///  @return operationsCount_ The number of operations
function operationsCount() public view returns (uint256 operationsCount_) {
    operationsCount_ = _operationsIdSet.length();
    return operationsCount_;
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

## State Variable Reads

- **_operationsIdSet** (`struct EnumerableSet.Bytes32Set`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: TimelockControllerEnumerable.operationsCount() (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
  └─ [1] ⚙️ FUNCTION: EnumerableSet.length(struct EnumerableSet.Bytes32Set) (NodeID: 1)
      💬 Args: [_operationsIdSet]
      👁️  Def: internal
    └─ [2] ⚙️ FUNCTION: EnumerableSet._length(struct EnumerableSet.Set) (NodeID: 2)
        💬 Args: [set._inner]
        👁️  Def: private
```

## Documentation

### Function Documentation

@dev Return the number of operations from the set
 @return operationsCount_ The number of operations
