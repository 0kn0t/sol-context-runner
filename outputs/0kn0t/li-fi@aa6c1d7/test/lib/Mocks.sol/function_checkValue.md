# Function: checkValue()

**Contract**: [test/lib/Mocks.sol/contract_ValueChecker.md]

## Metadata

- **Contract**: ValueChecker
- **Signature**: `checkValue()`
- **Visibility**: external
- **Source Range**: 7148:121:41

## Implementation

```solidity
/// @notice Store and return msg.value.
///  @dev Example: uint256 value = checker.checkValue{value: 1 ether}();
function checkValue() external payable returns (uint256) {
    lastValue = msg.value;
    return msg.value;
}
```

## State Variable Writes

- **lastValue** (`uint256`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: ValueChecker.checkValue() (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: external
```

## Documentation

### Function Documentation

@notice Store and return msg.value.
 @dev Example: uint256 value = checker.checkValue{value: 1 ether}();
