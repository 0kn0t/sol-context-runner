# Function: constructor(string)

**Contract**: [test/lib/Mocks.sol/contract_Reverter.md]

## Metadata

- **Contract**: Reverter
- **Signature**: `constructor(string)`
- **Visibility**: public
- **Source Range**: 4827:68:41

## Implementation

```solidity
/// @notice Configure revert behavior.
///  @param _reason Revert reason string.
constructor(string memory _reason) {
    reason = _reason;
}
```

## State Variable Writes

- **reason** (`string`)

## Call Tree

```
┌─ [0] 🏗️ CONSTRUCTOR: Reverter.constructor(string) (NodeID: 0)
    💬 Args: [no args]
    🏗️  Contract: Reverter
```

## Documentation

### Function Documentation

@notice Configure revert behavior.
 @param _reason Revert reason string.
