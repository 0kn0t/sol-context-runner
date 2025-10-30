# Function: assertEqual(uint256,uint256)

**Contract**: [src/InvariantChecker.sol/contract_InvariantChecker.md]

## Metadata

- **Contract**: InvariantChecker
- **Signature**: `assertEqual(uint256,uint256)`
- **Visibility**: external
- **Source Range**: 787:113:27

## Implementation

```solidity
///  @notice Asserts that two values are equal
function assertEqual(uint256 a, uint256 b) external pure {
    if (a != b) revert AssertEqFailed(a, b);
}
```

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: InvariantChecker.assertEqual(uint256,uint256) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: external
```

## Documentation

### Function Documentation

 @notice Asserts that two values are equal
