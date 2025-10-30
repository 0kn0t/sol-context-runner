# Function: hashOperation(address,uint256,bytes,bytes32,bytes32)

**Contract**: [lib/openzeppelin-community-contracts/contracts/governance/TimelockControllerEnumerable.sol/contract_TimelockControllerEnumerable.md]

## Metadata

- **Contract**: TimelockControllerEnumerable
- **Signature**: `hashOperation(address,uint256,bytes,bytes32,bytes32)`
- **Visibility**: public
- **Source Range**: 8142:279:72
- **Inherited From**: TimelockController

## Implementation

```solidity
///  @dev Returns the identifier of an operation containing a single
///  transaction.
function hashOperation(address target, uint256 value, bytes calldata data, bytes32 predecessor, bytes32 salt) virtual public pure returns (bytes32) {
    return keccak256(abi.encode(target, value, data, predecessor, salt));
}
```

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: TimelockController.hashOperation(address,uint256,bytes,bytes32,bytes32) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
```

## Documentation

### Function Documentation

 @dev Returns the identifier of an operation containing a single
 transaction.
