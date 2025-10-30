# Function: getMinDelay()

**Contract**: [lib/openzeppelin-community-contracts/contracts/governance/TimelockControllerEnumerable.sol/contract_TimelockControllerEnumerable.md]

## Metadata

- **Contract**: TimelockControllerEnumerable
- **Signature**: `getMinDelay()`
- **Visibility**: public
- **Source Range**: 7935:94:72
- **Inherited From**: TimelockController

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
