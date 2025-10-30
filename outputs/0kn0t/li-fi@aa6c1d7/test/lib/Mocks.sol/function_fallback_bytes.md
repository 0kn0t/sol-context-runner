# Function: fallback(bytes)

**Contract**: [test/lib/Mocks.sol/contract_EchoContract.md]

## Metadata

- **Contract**: EchoContract
- **Signature**: `fallback(bytes)`
- **Visibility**: external
- **Source Range**: 441:97:41

## Implementation

```solidity
/// @notice Return exact calldata received.
///  @dev Example: bytes memory result = echo.echo(data);
fallback(bytes calldata) external payable returns (bytes memory) {
    return msg.data;
}
```

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: EchoContract.fallback(bytes) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: external
```

## Documentation

### Function Documentation

@notice Return exact calldata received.
 @dev Example: bytes memory result = echo.echo(data);
