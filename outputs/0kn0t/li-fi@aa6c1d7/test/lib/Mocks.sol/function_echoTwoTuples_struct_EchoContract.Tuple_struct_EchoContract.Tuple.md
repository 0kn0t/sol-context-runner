# Function: echoTwoTuples(struct EchoContract.Tuple,struct EchoContract.Tuple)

**Contract**: [test/lib/Mocks.sol/contract_EchoContract.md]

## Metadata

- **Contract**: EchoContract
- **Signature**: `echoTwoTuples(struct EchoContract.Tuple,struct EchoContract.Tuple)`
- **Visibility**: external
- **Source Range**: 3726:202:41

## Implementation

```solidity
/// @notice Return two tuples unchanged.
///  @dev Example: (Tuple memory, Tuple memory) = target.echoTwoTuples(tup1, tup2);
function echoTwoTuples(Tuple calldata tup1, Tuple calldata tup2) external pure returns (Tuple memory, Tuple memory) {
    return (tup1, tup2);
}
```

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: EchoContract.echoTwoTuples(struct EchoContract.Tuple,struct EchoContract.Tuple) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: external
```

## Documentation

### Function Documentation

@notice Return two tuples unchanged.
 @dev Example: (Tuple memory, Tuple memory) = target.echoTwoTuples(tup1, tup2);
