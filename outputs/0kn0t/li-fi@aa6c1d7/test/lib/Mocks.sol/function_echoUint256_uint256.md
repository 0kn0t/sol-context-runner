# Function: echoUint256(uint256)

**Contract**: [test/lib/Mocks.sol/contract_EchoContract.md]

## Metadata

- **Contract**: EchoContract
- **Signature**: `echoUint256(uint256)`
- **Visibility**: public
- **Source Range**: 1799:95:41

## Implementation

```solidity
/// @notice Return exact uint256 received.
///  @dev Example: uint256 result = target.echoUint256(42);
function echoUint256(uint256 value) public pure returns (uint256) {
    return value;
}
```

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: EchoContract.echoUint256(uint256) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
```

## Documentation

### Function Documentation

@notice Return exact uint256 received.
 @dev Example: uint256 result = target.echoUint256(42);
