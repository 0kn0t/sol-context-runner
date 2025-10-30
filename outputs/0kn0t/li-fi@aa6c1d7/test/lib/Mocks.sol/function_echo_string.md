# Function: echo(string)

**Contract**: [test/lib/Mocks.sol/contract_EchoContract.md]

## Metadata

- **Contract**: EchoContract
- **Signature**: `echo(string)`
- **Visibility**: external
- **Source Range**: 743:102:41

## Implementation

```solidity
/// @notice Return exact string received.
///  @dev Example: string memory result = echo.echo("hello");
function echo(string memory input) external pure returns (string memory) {
    return input;
}
```

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: EchoContract.echo(string) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: external
```

## Documentation

### Function Documentation

@notice Return exact string received.
 @dev Example: string memory result = echo.echo("hello");
