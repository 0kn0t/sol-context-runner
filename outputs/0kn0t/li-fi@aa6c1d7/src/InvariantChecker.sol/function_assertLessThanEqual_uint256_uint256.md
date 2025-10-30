# Function: assertLessThanEqual(uint256,uint256)

**Contract**: [src/InvariantChecker.sol/contract_InvariantChecker.md]

## Metadata

- **Contract**: InvariantChecker
- **Signature**: `assertLessThanEqual(uint256,uint256)`
- **Visibility**: external
- **Source Range**: 1612:121:27

## Implementation

```solidity
///  @notice Asserts that the first value is less than or equal to the second
function assertLessThanEqual(uint256 a, uint256 b) external pure {
    if (a > b) revert AssertLteFailed(a, b);
}
```

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: InvariantChecker.assertLessThanEqual(uint256,uint256) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: external
```

## Documentation

### Function Documentation

 @notice Asserts that the first value is less than or equal to the second
