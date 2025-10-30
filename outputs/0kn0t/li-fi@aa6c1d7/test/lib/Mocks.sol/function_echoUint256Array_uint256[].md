# Function: echoUint256Array(uint256[])

**Contract**: [test/lib/Mocks.sol/contract_EchoContract.md]

## Metadata

- **Contract**: EchoContract
- **Signature**: `echoUint256Array(uint256[])`
- **Visibility**: public
- **Source Range**: 3001:114:41

## Implementation

```solidity
/// @notice Return uint256 array unchanged.
///  @dev Example: uint256[] memory result = target.echoUint256Array(values);
function echoUint256Array(uint256[] memory arr) public pure returns (uint256[] memory) {
    return arr;
}
```

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: EchoContract.echoUint256Array(uint256[]) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
```

## Documentation

### Function Documentation

@notice Return uint256 array unchanged.
 @dev Example: uint256[] memory result = target.echoUint256Array(values);
