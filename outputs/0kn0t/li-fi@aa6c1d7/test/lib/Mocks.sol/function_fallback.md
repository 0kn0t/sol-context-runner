# Function: fallback()

**Contract**: [test/lib/Mocks.sol/contract_Reverter.md]

## Metadata

- **Contract**: Reverter
- **Signature**: `fallback()`
- **Visibility**: external
- **Source Range**: 4978:59:41

## Implementation

```solidity
/// @notice Always reverts.
///  @dev Example: rev.fail(); // reverts
fallback() external payable {
    revert(reason);
}
```

## State Variable Reads

- **reason** (`string`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: Reverter.fallback() (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: external
```

## Documentation

### Function Documentation

@notice Always reverts.
 @dev Example: rev.fail(); // reverts
