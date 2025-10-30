# Function: echoStringArray(string[])

**Contract**: [test/lib/Mocks.sol/contract_EchoContract.md]

## Metadata

- **Contract**: EchoContract
- **Signature**: `echoStringArray(string[])`
- **Visibility**: public
- **Source Range**: 3248:111:41

## Implementation

```solidity
/// @notice Return string array unchanged.
///  @dev Example: string[] memory result = target.echoStringArray(strings);
function echoStringArray(string[] memory arr) public pure returns (string[] memory) {
    return arr;
}
```

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: EchoContract.echoStringArray(string[]) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
```

## Documentation

### Function Documentation

@notice Return string array unchanged.
 @dev Example: string[] memory result = target.echoStringArray(strings);
