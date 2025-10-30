# Function: echoTuple(struct EchoContract.Tuple)

**Contract**: [test/lib/Mocks.sol/contract_EchoContract.md]

## Metadata

- **Contract**: EchoContract
- **Signature**: `echoTuple(struct EchoContract.Tuple)`
- **Visibility**: external
- **Source Range**: 3485:103:41

## Implementation

```solidity
/// @notice Return tuple unchanged.
///  @dev Example: EchoContract.Tuple memory result = target.echoTuple(tup);
function echoTuple(Tuple calldata tup) external pure returns (Tuple memory) {
    return tup;
}
```

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: EchoContract.echoTuple(struct EchoContract.Tuple) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: external
```

## Documentation

### Function Documentation

@notice Return tuple unchanged.
 @dev Example: EchoContract.Tuple memory result = target.echoTuple(tup);
