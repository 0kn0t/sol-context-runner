# Function: hashOperationBatch(address[],uint256[],bytes[],bytes32,bytes32)

**Contract**: [lib/openzeppelin-contracts/contracts/governance/TimelockController.sol/contract_TimelockController.md]

## Metadata

- **Contract**: TimelockController
- **Signature**: `hashOperationBatch(address[],uint256[],bytes[],bytes32,bytes32)`
- **Visibility**: public
- **Source Range**: 8537:320:72

## Implementation

```solidity
///  @dev Returns the identifier of an operation containing a batch of
///  transactions.
function hashOperationBatch(address[] calldata targets, uint256[] calldata values, bytes[] calldata payloads, bytes32 predecessor, bytes32 salt) virtual public pure returns (bytes32) {
    return keccak256(abi.encode(targets, values, payloads, predecessor, salt));
}
```

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: TimelockController.hashOperationBatch(address[],uint256[],bytes[],bytes32,bytes32) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
```

## Documentation

### Function Documentation

 @dev Returns the identifier of an operation containing a batch of
 transactions.
