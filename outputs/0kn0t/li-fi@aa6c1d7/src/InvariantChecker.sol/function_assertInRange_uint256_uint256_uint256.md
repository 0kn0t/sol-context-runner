# Function: assertInRange(uint256,uint256,uint256)

**Contract**: [src/InvariantChecker.sol/contract_InvariantChecker.md]

## Metadata

- **Contract**: InvariantChecker
- **Signature**: `assertInRange(uint256,uint256,uint256)`
- **Visibility**: external
- **Source Range**: 2200:168:27

## Implementation

```solidity
///  @notice Asserts that a value is within an inclusive range
///  @param value The value to check
///  @param min The minimum allowed value (inclusive)
///  @param max The maximum allowed value (inclusive)
function assertInRange(uint256 value, uint256 min, uint256 max) external pure {
    if ((value < min) || (value > max)) revert AssertRangeFailed(value, min, max);
}
```

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: InvariantChecker.assertInRange(uint256,uint256,uint256) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: external
```

## Documentation

### Function Documentation

 @notice Asserts that a value is within an inclusive range
 @param value The value to check
 @param min The minimum allowed value (inclusive)
 @param max The maximum allowed value (inclusive)
