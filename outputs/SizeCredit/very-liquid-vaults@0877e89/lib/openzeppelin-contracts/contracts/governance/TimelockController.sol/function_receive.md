# Function: receive()

**Contract**: [lib/openzeppelin-contracts/contracts/governance/TimelockController.sol/contract_TimelockController.md]

## Metadata

- **Contract**: TimelockController
- **Signature**: `receive()`
- **Visibility**: external
- **Source Range**: 5549:37:72

## Implementation

```solidity
///  @dev Contract might receive/hold ETH as part of the maintenance process.
receive() virtual external payable {}
```

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: TimelockController.receive() (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: external
```

## Documentation

### Function Documentation

 @dev Contract might receive/hold ETH as part of the maintenance process.
