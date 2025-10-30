# Function: doesUserHaveRole(address,uint8)

**Contract**: [src/Dependencies/RolesAuthority.sol/contract_RolesAuthority.md]

## Metadata

- **Contract**: RolesAuthority
- **Signature**: `doesUserHaveRole(address,uint8)`
- **Visibility**: public
- **Source Range**: 1600:157:37

## Implementation

```solidity
function doesUserHaveRole(address user, uint8 role) virtual public view returns (bool) {
    return ((uint256(getUserRoles[user]) >> role) & 1) != 0;
}
```

## State Variable Reads

- **getUserRoles** (`mapping(address => bytes32)`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: RolesAuthority.doesUserHaveRole(address,uint8) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
```
