# Function: echoStrings(string,string)

**Contract**: [test/lib/Mocks.sol/contract_EchoContract.md]

## Metadata

- **Contract**: EchoContract
- **Signature**: `echoStrings(string,string)`
- **Visibility**: public
- **Source Range**: 2474:138:41

## Implementation

```solidity
/// @notice Return two strings unchanged.
///  @dev Example: (string memory, string memory) = target.echoStrings("a", "b");
function echoStrings(string memory s, string memory s2) public pure returns (string memory, string memory) {
    return (s, s2);
}
```

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: EchoContract.echoStrings(string,string) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
```

## Documentation

### Function Documentation

@notice Return two strings unchanged.
 @dev Example: (string memory, string memory) = target.echoStrings("a", "b");
