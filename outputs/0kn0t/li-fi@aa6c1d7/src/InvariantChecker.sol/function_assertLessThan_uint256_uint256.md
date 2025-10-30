# Function: assertLessThan(uint256,uint256)

**Contract**: [src/InvariantChecker.sol/contract_InvariantChecker.md]

## Metadata

- **Contract**: InvariantChecker
- **Signature**: `assertLessThan(uint256,uint256)`
- **Visibility**: external
- **Source Range**: 1182:116:27

## Implementation

```solidity
///  @notice Asserts that the first value is less than the second
function assertLessThan(uint256 a, uint256 b) external pure {
    if (a >= b) revert AssertLtFailed(a, b);
}
```

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: InvariantChecker.assertLessThan(uint256,uint256) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: external
```

## Documentation

### Function Documentation

 @notice Asserts that the first value is less than the second
