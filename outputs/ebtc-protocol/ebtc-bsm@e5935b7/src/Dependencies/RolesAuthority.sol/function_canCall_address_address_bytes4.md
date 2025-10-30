# Function: canCall(address,address,bytes4)

**Contract**: [src/Dependencies/RolesAuthority.sol/contract_RolesAuthority.md]

## Metadata

- **Contract**: RolesAuthority
- **Signature**: `canCall(address,address,bytes4)`
- **Visibility**: public
- **Source Range**: 2652:490:37

## Implementation

```solidity
/// @notice A user can call a given function signature on a given target address if:
/// - The capability has not been burned
/// - That capability is public, or the user has a role that has been granted the capability to call the function
function canCall(address user, address target, bytes4 functionSig) virtual override public view returns (bool) {
    CapabilityFlag flag = capabilityFlag[target][functionSig];
    if (flag == CapabilityFlag.Burned) {
        return false;
    } else if (flag == CapabilityFlag.Public) {
        return true;
    } else {
        return bytes32(0) != (getUserRoles[user] & getRolesWithCapability[target][functionSig]);
    }
}
```

## State Variable Reads

- **capabilityFlag** (`mapping(address => mapping(bytes4 => enum IRolesAuthority.CapabilityFlag))`)
- **getUserRoles** (`mapping(address => bytes32)`)
- **getRolesWithCapability** (`mapping(address => mapping(bytes4 => bytes32))`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: RolesAuthority.canCall(address,address,bytes4) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
```

## Documentation

### Function Documentation

@notice A user can call a given function signature on a given target address if:
- The capability has not been burned
- That capability is public, or the user has a role that has been granted the capability to call the function
