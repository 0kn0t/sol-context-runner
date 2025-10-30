# Function: canProcess(uint256,address)

**Contract**: [src/DummyConstraint.sol/contract_DummyConstraint.md]

## Metadata

- **Contract**: DummyConstraint
- **Signature**: `canProcess(uint256,address)`
- **Visibility**: external
- **Source Range**: 252:128:38

## Implementation

```solidity
/// @notice Returns true
function canProcess(uint256 _amount, address _bsm) external view returns (bool, bytes memory) {
    return (true, "");
}
```

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: DummyConstraint.canProcess(uint256,address) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: external
```

## Documentation

### Function Documentation

@notice Returns true
