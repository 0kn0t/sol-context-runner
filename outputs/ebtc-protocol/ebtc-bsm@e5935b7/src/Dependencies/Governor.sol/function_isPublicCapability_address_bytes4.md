# Function: isPublicCapability(address,bytes4)

**Contract**: [src/Dependencies/Governor.sol/contract_Governor.md]

## Metadata

- **Contract**: Governor
- **Signature**: `isPublicCapability(address,bytes4)`
- **Visibility**: public
- **Source Range**: 2009:175:37
- **Inherited From**: RolesAuthority

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
