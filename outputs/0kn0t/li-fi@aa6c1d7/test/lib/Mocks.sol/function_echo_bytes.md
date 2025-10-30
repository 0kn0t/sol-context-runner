# Function: echo(bytes)

**Contract**: [test/lib/Mocks.sol/contract_EchoContract.md]

## Metadata

- **Contract**: EchoContract
- **Signature**: `echo(bytes)`
- **Visibility**: external
- **Source Range**: 957:100:41

## Implementation

```solidity
/// @notice Return exact bytes received.
///  @dev Example: bytes memory result = echo.echo(data);
function echo(bytes memory input) external pure returns (bytes memory) {
    return input;
}
```

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: EchoContract.echo(bytes) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: external
```

## Documentation

### Function Documentation

@notice Return exact bytes received.
 @dev Example: bytes memory result = echo.echo(data);
