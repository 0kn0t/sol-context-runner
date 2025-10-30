# Function: echoString(string)

**Contract**: [test/lib/Mocks.sol/contract_EchoContract.md]

## Metadata

- **Contract**: EchoContract
- **Signature**: `echoString(string)`
- **Visibility**: public
- **Source Range**: 2239:98:41

## Implementation

```solidity
/// @notice Return exact string received.
///  @dev Example: string memory result = target.echoString("hello");
function echoString(string memory s) public pure returns (string memory) {
    return s;
}
```

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: EchoContract.echoString(string) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
```

## Documentation

### Function Documentation

@notice Return exact string received.
 @dev Example: string memory result = target.echoString("hello");
