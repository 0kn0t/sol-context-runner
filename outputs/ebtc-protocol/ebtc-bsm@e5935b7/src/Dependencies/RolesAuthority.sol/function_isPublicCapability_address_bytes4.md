# Function: isPublicCapability(address,bytes4)

**Contract**: [src/Dependencies/RolesAuthority.sol/contract_RolesAuthority.md]

## Metadata

- **Contract**: RolesAuthority
- **Signature**: `isPublicCapability(address,bytes4)`
- **Visibility**: public
- **Source Range**: 2009:175:37

## Implementation

```solidity
function isPublicCapability(address target, bytes4 functionSig) public view returns (bool) {
    return capabilityFlag[target][functionSig] == CapabilityFlag.Public;
}
```

## State Variable Reads

- **capabilityFlag** (`mapping(address => mapping(bytes4 => enum IRolesAuthority.CapabilityFlag))`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: RolesAuthority.isPublicCapability(address,bytes4) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
```
