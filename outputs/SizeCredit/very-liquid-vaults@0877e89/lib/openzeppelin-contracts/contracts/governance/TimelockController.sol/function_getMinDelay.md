# Function: getMinDelay()

**Contract**: [lib/openzeppelin-contracts/contracts/governance/TimelockController.sol/contract_TimelockController.md]

## Metadata

- **Contract**: TimelockController
- **Signature**: `getMinDelay()`
- **Visibility**: public
- **Source Range**: 7935:94:72

## Implementation

```solidity
///  @dev Returns the minimum delay in seconds for an operation to become valid.
///  This value can be changed by executing an operation that calls `updateDelay`.
function getMinDelay() virtual public view returns (uint256) {
    return _minDelay;
}
```

## State Variable Reads

- **_minDelay** (`uint256`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: TimelockController.getMinDelay() (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
```

## Documentation

### Function Documentation

 @dev Returns the minimum delay in seconds for an operation to become valid.
 This value can be changed by executing an operation that calls `updateDelay`.
