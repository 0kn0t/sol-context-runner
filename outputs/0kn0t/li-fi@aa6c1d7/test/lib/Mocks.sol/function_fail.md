# Function: fail()

**Contract**: [test/lib/Mocks.sol/contract_RevertingTarget.md]

## Metadata

- **Contract**: RevertingTarget
- **Signature**: `fail()`
- **Visibility**: public
- **Source Range**: 5743:94:41

## Implementation

```solidity
/// @notice Always reverts with error.
///  @dev Example: target.fail(); // reverts
function fail() public pure {
    revert AlwaysFails("Execution is meant to fail.");
}
```

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: RevertingTarget.fail() (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
```

## Documentation

### Function Documentation

@notice Always reverts with error.
 @dev Example: target.fail(); // reverts
