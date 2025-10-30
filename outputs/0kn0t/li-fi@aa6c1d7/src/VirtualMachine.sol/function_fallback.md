# Function: fallback()

**Contract**: [src/VirtualMachine.sol/contract_VirtualMachine.md]

## Metadata

- **Contract**: VirtualMachine
- **Signature**: `fallback()`
- **Visibility**: external
- **Source Range**: 8301:31:35

## Implementation

```solidity
/// @dev Accept direct ether transfers for value call operations
fallback() external payable {}
```

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: VirtualMachine.fallback() (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: external
```

## Documentation

### Function Documentation

@dev Accept direct ether transfers for value call operations
