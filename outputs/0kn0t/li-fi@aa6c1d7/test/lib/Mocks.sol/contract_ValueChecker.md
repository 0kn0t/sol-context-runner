# Contract: ValueChecker

## Metadata

- **Name**: ValueChecker
- **Type**: Contract
- **Path**: test/lib/Mocks.sol
- **Documentation**: @notice Checks msg.value and returns it.
   @dev Example: ValueChecker checker = new ValueChecker();

## State Variables

### lastValue

```solidity
uint256 public lastValue
```

## Public/External Functions

### checkValue()

- **Signature**: `checkValue()`
- **Visibility**: external
- **Source Range**: 7148:121:41
- **Details**: [function_checkValue.md](./function_checkValue.md)

**Signature:**
```solidity
/// @notice Store and return msg.value.
///  @dev Example: uint256 value = checker.checkValue{value: 1 ether}();
function checkValue() external payable returns (uint256);
```
