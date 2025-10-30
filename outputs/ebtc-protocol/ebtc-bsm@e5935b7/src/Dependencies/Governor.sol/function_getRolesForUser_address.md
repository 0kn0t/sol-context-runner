# Function: getRolesForUser(address)

**Contract**: [src/Dependencies/Governor.sol/contract_Governor.md]

## Metadata

- **Contract**: Governor
- **Signature**: `getRolesForUser(address)`
- **Visibility**: external
- **Source Range**: 3015:916:28

## Implementation

```solidity
/// @notice Returns a list of roles that an address has.
///  @dev This function searches all roles and checks if they are assigned to the given user.
///  @dev Intended for off-chain utility only due to inefficiency.
///  @param user The address of the user.
///  @return rolesForUser An array of role IDs that the user has.
function getRolesForUser(address user) external view returns (uint8[] memory rolesForUser) {
    uint256 count;
    for (uint8 i = 0; i <= type(uint8).max; ) {
        if (doesUserHaveRole(user, i)) {
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
        rolesForUser = new uint8[](count);
        for (uint8 i = 0; i <= type(uint8).max; ) {
            if (doesUserHaveRole(user, i)) {
                rolesForUser[j] = i;
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

## Related Implementations

### doesUserHaveRole(address,uint8)

- **Kind**: internal
- **Source**: 1600:157:37
- **Link**: `src/Dependencies/RolesAuthority.sol:RolesAuthority:doesUserHaveRole(address,uint8)`

```solidity
function doesUserHaveRole(address user, uint8 role) virtual public view returns (bool) {
    return ((uint256(getUserRoles[user]) >> role) & 1) != 0;
}
```

## State Variable Reads

- **getUserRoles** (`mapping(address => bytes32)`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: Governor.getRolesForUser(address) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: external
  ├─ [1] ⚙️ FUNCTION: RolesAuthority.doesUserHaveRole(address,uint8) (NodeID: 1)
  │   💬 Args: [user, i]
  │   👁️  Def: public
  └─ [1] ⚙️ FUNCTION: RolesAuthority.doesUserHaveRole(address,uint8) (NodeID: 2)
      💬 Args: [user, i]
      👁️  Def: public
```

## Documentation

### Function Documentation

@notice Returns a list of roles that an address has.
 @dev This function searches all roles and checks if they are assigned to the given user.
 @dev Intended for off-chain utility only due to inefficiency.
 @param user The address of the user.
 @return rolesForUser An array of role IDs that the user has.
