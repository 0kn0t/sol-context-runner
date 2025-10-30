# Function: echoBytes(bytes)

**Contract**: [test/lib/Mocks.sol/contract_EchoContract.md]

## Metadata

- **Contract**: EchoContract
- **Signature**: `echoBytes(bytes)`
- **Visibility**: public
- **Source Range**: 2013:101:41

## Implementation

```solidity
/// @notice Return exact bytes received.
///  @dev Example: bytes memory result = target.echoBytes(data);
function echoBytes(bytes memory data) public pure returns (bytes memory) {
    return data;
}
```

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: EchoContract.echoBytes(bytes) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
```

## Documentation

### Function Documentation

@notice Return exact bytes received.
 @dev Example: bytes memory result = target.echoBytes(data);
