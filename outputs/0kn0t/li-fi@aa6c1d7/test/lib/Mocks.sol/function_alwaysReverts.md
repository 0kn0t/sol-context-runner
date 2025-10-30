# Function: alwaysReverts()

**Contract**: [test/lib/Mocks.sol/contract_RevertingTarget.md]

## Metadata

- **Contract**: RevertingTarget
- **Signature**: `alwaysReverts()`
- **Visibility**: external
- **Source Range**: 5945:80:41

## Implementation

```solidity
/// @notice Always reverts with message.
///  @dev Example: target.alwaysReverts(); // reverts
function alwaysReverts() external pure {
    revert("Always reverts");
}
```

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: RevertingTarget.alwaysReverts() (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: external
```

## Documentation

### Function Documentation

@notice Always reverts with message.
 @dev Example: target.alwaysReverts(); // reverts
