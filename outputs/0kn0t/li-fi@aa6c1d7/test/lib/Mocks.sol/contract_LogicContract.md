# Contract: LogicContract

## Metadata

- **Name**: LogicContract
- **Type**: Contract
- **Path**: test/lib/Mocks.sol
- **Documentation**: @notice Stateful contract for testing DELEGATECALL.
   @dev Example: LogicContract logic = new LogicContract();

## State Variables

### x

```solidity
uint256 public x
```

## Public/External Functions

### setX(uint256)

- **Signature**: `setX(uint256)`
- **Visibility**: public
- **Source Range**: 6274:56:41
- **Details**: [function_setX_uint256.md](./function_setX_uint256.md)

**Signature:**
```solidity
/// @notice Set state variable x.
///  @dev Example: logic.setX(42);
function setX(uint256 _x) public;
```
