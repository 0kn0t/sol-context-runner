# Function: assertGreaterThanEqual(uint256,uint256)

**Contract**: [src/InvariantChecker.sol/contract_InvariantChecker.md]

## Metadata

- **Contract**: InvariantChecker
- **Signature**: `assertGreaterThanEqual(uint256,uint256)`
- **Visibility**: external
- **Source Range**: 1838:124:27

## Implementation

```solidity
///  @notice Asserts that the first value is greater than or equal to the second
function assertGreaterThanEqual(uint256 a, uint256 b) external pure {
    if (a < b) revert AssertGteFailed(a, b);
}
```

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: InvariantChecker.assertGreaterThanEqual(uint256,uint256) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: external
```

## Documentation

### Function Documentation

 @notice Asserts that the first value is greater than or equal to the second
