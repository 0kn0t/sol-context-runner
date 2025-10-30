# Function: assertGreaterThan(uint256,uint256)

**Contract**: [src/InvariantChecker.sol/contract_InvariantChecker.md]

## Metadata

- **Contract**: InvariantChecker
- **Signature**: `assertGreaterThan(uint256,uint256)`
- **Visibility**: external
- **Source Range**: 1391:119:27

## Implementation

```solidity
///  @notice Asserts that the first value is greater than the second
function assertGreaterThan(uint256 a, uint256 b) external pure {
    if (a <= b) revert AssertGtFailed(a, b);
}
```

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: InvariantChecker.assertGreaterThan(uint256,uint256) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: external
```

## Documentation

### Function Documentation

 @notice Asserts that the first value is greater than the second
