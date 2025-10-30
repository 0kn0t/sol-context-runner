# Function: getByteMapFromRoles(uint8[])

**Contract**: [src/Dependencies/Governor.sol/contract_Governor.md]

## Metadata

- **Contract**: Governor
- **Signature**: `getByteMapFromRoles(uint8[])`
- **Visibility**: public
- **Source Range**: 5273:256:28

## Implementation

```solidity
/// @notice Converts an array of role IDs to a byte map representation.
///  @param roleIds An array of role IDs.
///  @return A bytes32 value encoding the roles.
function getByteMapFromRoles(uint8[] memory roleIds) public pure returns (bytes32) {
    bytes32 _data;
    for (uint256 i = 0; i < roleIds.length; i++) {
        _data |= bytes32(1 << uint256(roleIds[i]));
    }
    return _data;
}
```

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: Governor.getByteMapFromRoles(uint8[]) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
```

## Documentation

### Function Documentation

@notice Converts an array of role IDs to a byte map representation.
 @param roleIds An array of role IDs.
 @return A bytes32 value encoding the roles.
