# Function: changeState(uint256)

**Contract**: [test/lib/Mocks.sol/contract_StateChanger.md]

## Metadata

- **Contract**: StateChanger
- **Signature**: `changeState(uint256)`
- **Visibility**: external
- **Source Range**: 7536:81:41

## Implementation

```solidity
/// @notice Change state to new value.
///  @dev Example: changer.changeState(42);
function changeState(uint256 newState) external {
    state = newState;
}
```

## State Variable Writes

- **state** (`uint256`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: StateChanger.changeState(uint256) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: external
```

## Documentation

### Function Documentation

@notice Change state to new value.
 @dev Example: changer.changeState(42);
