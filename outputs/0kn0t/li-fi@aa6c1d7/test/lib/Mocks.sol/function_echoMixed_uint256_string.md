# Function: echoMixed(uint256,string)

**Contract**: [test/lib/Mocks.sol/contract_EchoContract.md]

## Metadata

- **Contract**: EchoContract
- **Signature**: `echoMixed(uint256,string)`
- **Visibility**: public
- **Source Range**: 2744:122:41

## Implementation

```solidity
/// @notice Return mixed types unchanged.
///  @dev Example: (uint256, string memory) = target.echoMixed(42, "hello");
function echoMixed(uint256 x, string memory s) public pure returns (uint256, string memory) {
    return (x, s);
}
```

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: EchoContract.echoMixed(uint256,string) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
```

## Documentation

### Function Documentation

@notice Return mixed types unchanged.
 @dev Example: (uint256, string memory) = target.echoMixed(42, "hello");
