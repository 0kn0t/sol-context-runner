# Function: getTimestamp(bytes32)

**Contract**: [lib/openzeppelin-contracts/contracts/governance/TimelockController.sol/contract_TimelockController.md]

## Metadata

- **Contract**: TimelockController
- **Signature**: `getTimestamp(bytes32)`
- **Visibility**: public
- **Source Range**: 7108:111:72

## Implementation

```solidity
///  @dev Returns the timestamp at which an operation becomes ready (0 for
///  unset operations, 1 for done operations).
function getTimestamp(bytes32 id) virtual public view returns (uint256) {
    return _timestamps[id];
}
```

## State Variable Reads

- **_timestamps** (`mapping(bytes32 => uint256)`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: TimelockController.getTimestamp(bytes32) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
```

## Documentation

### Function Documentation

 @dev Returns the timestamp at which an operation becomes ready (0 for
 unset operations, 1 for done operations).
