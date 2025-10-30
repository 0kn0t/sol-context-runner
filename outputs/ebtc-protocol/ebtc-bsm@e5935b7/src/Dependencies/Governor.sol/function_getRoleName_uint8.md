# Function: getRoleName(uint8)

**Contract**: [src/Dependencies/Governor.sol/contract_Governor.md]

## Metadata

- **Contract**: Governor
- **Signature**: `getRoleName(uint8)`
- **Visibility**: external
- **Source Range**: 6299:119:28

## Implementation

```solidity
/// @notice Retrieves the name associated with a role ID
///  @param role The role ID
///  @return roleName The name of the role
function getRoleName(uint8 role) external view returns (string memory roleName) {
    return roleNames[role];
}
```

## State Variable Reads

- **roleNames** (`mapping(uint8 => string)`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: Governor.getRoleName(uint8) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: external
```

## Documentation

### Function Documentation

@notice Retrieves the name associated with a role ID
 @param role The role ID
 @return roleName The name of the role
