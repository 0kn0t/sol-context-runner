# Function: echo(uint256,string[],uint256)

**Contract**: [test/lib/Mocks.sol/contract_EchoContract.md]

## Metadata

- **Contract**: EchoContract
- **Signature**: `echo(uint256,string[],uint256)`
- **Visibility**: external
- **Source Range**: 1211:236:41

## Implementation

```solidity
/// @notice Return exact tuple received.
///  @dev Example: (uint256, string[], uint256) memory result = echo.echo(value1, strings, value2);
function echo(uint256 value1, string[] memory strings, uint256 value2) external pure returns (uint256, string[] memory, uint256) {
    return (value1, strings, value2);
}
```

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: EchoContract.echo(uint256,string[],uint256) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: external
```

## Documentation

### Function Documentation

@notice Return exact tuple received.
 @dev Example: (uint256, string[], uint256) memory result = echo.echo(value1, strings, value2);
