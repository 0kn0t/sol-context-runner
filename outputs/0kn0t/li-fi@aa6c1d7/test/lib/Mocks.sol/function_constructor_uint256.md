# Function: constructor(uint256)

**Contract**: [test/lib/Mocks.sol/contract_ValueCheck.md]

## Metadata

- **Contract**: ValueCheck
- **Signature**: `constructor(uint256)`
- **Visibility**: public
- **Source Range**: 4238:83:41

## Implementation

```solidity
/// @notice Set expected value on deployment.
///  @param _expectedValue Required msg.value for calls.
constructor(uint256 _expectedValue) {
    expectedValue = _expectedValue;
}
```

## State Variable Writes

- **expectedValue** (`uint256`)

## Call Tree

```
┌─ [0] 🏗️ CONSTRUCTOR: ValueCheck.constructor(uint256) (NodeID: 0)
    💬 Args: [no args]
    🏗️  Contract: ValueCheck
```

## Documentation

### Function Documentation

@notice Set expected value on deployment.
 @param _expectedValue Required msg.value for calls.
