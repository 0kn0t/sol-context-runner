# Function: isOperationDone(bytes32)

**Contract**: [lib/openzeppelin-contracts/contracts/governance/TimelockController.sol/contract_TimelockController.md]

## Metadata

- **Contract**: TimelockController
- **Signature**: `isOperationDone(bytes32)`
- **Visibility**: public
- **Source Range**: 6828:132:72

## Implementation

```solidity
///  @dev Returns whether an operation is done or not.
function isOperationDone(bytes32 id) public view returns (bool) {
    return getOperationState(id) == OperationState.Done;
}
```

## Related Implementations

### getOperationState(bytes32)

- **Kind**: internal
- **Source**: 7278:460:72
- **Link**: `lib/openzeppelin-contracts/contracts/governance/TimelockController.sol:TimelockController:getOperationState(bytes32)`

```solidity
///  @dev Returns operation state.
function getOperationState(bytes32 id) virtual public view returns (OperationState) {
    uint256 timestamp = getTimestamp(id);
    if (timestamp == 0) {
        return OperationState.Unset;
    } else if (timestamp == _DONE_TIMESTAMP) {
        return OperationState.Done;
    } else if (timestamp > block.timestamp) {
        return OperationState.Waiting;
    } else {
        return OperationState.Ready;
    }
}
```

### getTimestamp(bytes32)

- **Kind**: internal
- **Source**: 7108:111:72
- **Link**: `lib/openzeppelin-contracts/contracts/governance/TimelockController.sol:TimelockController:getTimestamp(bytes32)`

```solidity
///  @dev Returns the timestamp at which an operation becomes ready (0 for
///  unset operations, 1 for done operations).
function getTimestamp(bytes32 id) virtual public view returns (uint256) {
    return _timestamps[id];
}
```

## State Variable Reads

- **_DONE_TIMESTAMP** (`uint256`)
- **_timestamps** (`mapping(bytes32 => uint256)`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: TimelockController.isOperationDone(bytes32) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
  └─ [1] ⚙️ FUNCTION: TimelockController.getOperationState(bytes32) (NodeID: 1)
      💬 Args: [id]
      👁️  Def: public
    └─ [2] ⚙️ FUNCTION: TimelockController.getTimestamp(bytes32) (NodeID: 2)
        💬 Args: [id]
        👁️  Def: public
```

## Documentation

### Function Documentation

 @dev Returns whether an operation is done or not.
