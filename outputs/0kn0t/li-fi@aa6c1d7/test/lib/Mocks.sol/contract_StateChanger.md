# Contract: StateChanger

## Metadata

- **Name**: StateChanger
- **Type**: Contract
- **Path**: test/lib/Mocks.sol
- **Documentation**: @notice Stateful contract for testing state changes.
   @dev Example: StateChanger changer = new StateChanger();

## State Variables

### state

```solidity
uint256 public state
```

## Public/External Functions

### changeState(uint256)

- **Signature**: `changeState(uint256)`
- **Visibility**: external
- **Source Range**: 7536:81:41
- **Details**: [function_changeState_uint256.md](./function_changeState_uint256.md)

**Signature:**
```solidity
/// @notice Change state to new value.
///  @dev Example: changer.changeState(42);
function changeState(uint256 newState) external;
```

### viewState()

- **Signature**: `viewState()`
- **Visibility**: external
- **Source Range**: 7725:82:41
- **Details**: [function_viewState.md](./function_viewState.md)

**Signature:**
```solidity
/// @notice View current state.
///  @dev Example: uint256 currentState = changer.viewState();
function viewState() external view returns (uint256);
```
