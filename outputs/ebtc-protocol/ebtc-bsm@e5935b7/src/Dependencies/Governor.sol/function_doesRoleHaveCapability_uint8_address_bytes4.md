# Function: doesRoleHaveCapability(uint8,address,bytes4)

**Contract**: [src/Dependencies/Governor.sol/contract_Governor.md]

## Metadata

- **Contract**: Governor
- **Signature**: `doesRoleHaveCapability(uint8,address,bytes4)`
- **Visibility**: public
- **Source Range**: 1763:240:37
- **Inherited From**: RolesAuthority

## Implementation

```solidity
function doesRoleHaveCapability(uint8 role, address target, bytes4 functionSig) virtual public view returns (bool) {
    return ((uint256(getRolesWithCapability[target][functionSig]) >> role) & 1) != 0;
}
```

## State Variable Reads

- **getRolesWithCapability** (`mapping(address => mapping(bytes4 => bytes32))`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: RolesAuthority.doesRoleHaveCapability(uint8,address,bytes4) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
```
