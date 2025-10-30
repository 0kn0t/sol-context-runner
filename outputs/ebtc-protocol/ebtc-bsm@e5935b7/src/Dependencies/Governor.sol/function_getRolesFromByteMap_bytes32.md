# Function: getRolesFromByteMap(bytes32)

**Contract**: [src/Dependencies/Governor.sol/contract_Governor.md]

## Metadata

- **Contract**: Governor
- **Signature**: `getRolesFromByteMap(bytes32)`
- **Visibility**: public
- **Source Range**: 4148:946:28

## Implementation

```solidity
/// @notice Converts a byte map representation to an array of role IDs.
///  @param byteMap The bytes32 value encoding the roles.
///  @return roleIds An array of role IDs extracted from the byte map.
function getRolesFromByteMap(bytes32 byteMap) public pure returns (uint8[] memory roleIds) {
    uint256 count;
    for (uint8 i = 0; i <= type(uint8).max; ) {
        bool roleEnabled = (uint256(byteMap >> i) & 1) != 0;
        if (roleEnabled) {
            count += 1;
        }
        if (i < type(uint8).max) {
            i = i + 1;
        } else {
            break;
        }
    }
    if (count > 0) {
        uint256 j = 0;
        roleIds = new uint8[](count);
        for (uint8 i = 0; i <= type(uint8).max; ) {
            bool roleEnabled = (uint256(byteMap >> i) & 1) != 0;
            if (roleEnabled) {
                roleIds[j] = i;
                j++;
            }
            if (i < type(uint8).max) {
                i = i + 1;
            } else {
                break;
            }
        }
    }
}
```

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: Governor.getRolesFromByteMap(bytes32) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
```

## Documentation

### Function Documentation

@notice Converts a byte map representation to an array of role IDs.
 @param byteMap The bytes32 value encoding the roles.
 @return roleIds An array of role IDs extracted from the byte map.
