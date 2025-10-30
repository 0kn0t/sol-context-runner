# Function: echo(uint256[])

**Contract**: [test/lib/Mocks.sol/contract_EchoContract.md]

## Metadata

- **Contract**: EchoContract
- **Signature**: `echo(uint256[])`
- **Visibility**: external
- **Source Range**: 1573:110:41

## Implementation

```solidity
/// @notice Return exact uint256 array received.
///  @dev Example: uint256[] memory result = echo.echo(values);
function echo(uint256[] memory values) external pure returns (uint256[] memory) {
    return values;
}
```

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: EchoContract.echo(uint256[]) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: external
```

## Documentation

### Function Documentation

@notice Return exact uint256 array received.
 @dev Example: uint256[] memory result = echo.echo(values);
