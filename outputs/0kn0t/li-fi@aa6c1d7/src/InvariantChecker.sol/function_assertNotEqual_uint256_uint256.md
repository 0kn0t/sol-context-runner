# Function: assertNotEqual(uint256,uint256)

**Contract**: [src/InvariantChecker.sol/contract_InvariantChecker.md]

## Metadata

- **Contract**: InvariantChecker
- **Signature**: `assertNotEqual(uint256,uint256)`
- **Visibility**: external
- **Source Range**: 975:117:27

## Implementation

```solidity
///  @notice Asserts that two values are not equal
function assertNotEqual(uint256 a, uint256 b) external pure {
    if (a == b) revert AssertNeqFailed(a, b);
}
```

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: InvariantChecker.assertNotEqual(uint256,uint256) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: external
```

## Documentation

### Function Documentation

 @notice Asserts that two values are not equal
