# Function: viewState()

**Contract**: [test/lib/Mocks.sol/contract_StateChanger.md]

## Metadata

- **Contract**: StateChanger
- **Signature**: `viewState()`
- **Visibility**: external
- **Source Range**: 7725:82:41

## Implementation

```solidity
/// @notice View current state.
///  @dev Example: uint256 currentState = changer.viewState();
function viewState() external view returns (uint256) {
    return state;
}
```

## State Variable Reads

- **state** (`uint256`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: StateChanger.viewState() (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: external
```

## Documentation

### Function Documentation

@notice View current state.
 @dev Example: uint256 currentState = changer.viewState();
