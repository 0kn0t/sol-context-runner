# Function: setPublicCapability(address,bytes4,bool)

**Contract**: [src/Dependencies/RolesAuthority.sol/contract_RolesAuthority.md]

## Metadata

- **Contract**: RolesAuthority
- **Signature**: `setPublicCapability(address,bytes4,bool)`
- **Visibility**: public
- **Source Range**: 3522:558:37

## Implementation

```solidity
/// @notice Set a capability flag as public, meaning any account can call it. Or revoke this capability.
///  @dev A capability cannot be made public if it has been burned.
function setPublicCapability(address target, bytes4 functionSig, bool enabled) virtual public requiresAuth() {
    require(capabilityFlag[target][functionSig] != CapabilityFlag.Burned, "RolesAuthority: Capability Burned");
    if (enabled) {
        capabilityFlag[target][functionSig] = CapabilityFlag.Public;
    } else {
        capabilityFlag[target][functionSig] = CapabilityFlag.None;
    }
    emit PublicCapabilityUpdated(target, functionSig, enabled);
}
```

## Related Implementations

### requiresAuth()

- **Kind**: modifier
- **Source**: 895:125:24
- **Link**: `src/Dependencies/Auth.sol:Auth:requiresAuth()`

```solidity
modifier requiresAuth() virtual {
    require(isAuthorized(msg.sender, msg.sig), "Auth: UNAUTHORIZED");
    _;
}
```

### isAuthorized(address,bytes4)

- **Kind**: internal
- **Source**: 1026:564:24
- **Link**: `src/Dependencies/Auth.sol:Auth:isAuthorized(address,bytes4)`

```solidity
function isAuthorized(address user, bytes4 functionSig) virtual internal view returns (bool) {
    Authority auth = authority;
    return ((address(auth) != address(0)) && auth.canCall(user, address(this), functionSig)) || (user == owner);
}
```

## State Variable Reads

- **capabilityFlag** (`mapping(address => mapping(bytes4 => enum IRolesAuthority.CapabilityFlag))`)
- **authority** (`contract Authority`) [src/Dependencies/Authority.sol/interface_Authority.md]
- **owner** (`address`)

## State Variable Writes

- **capabilityFlag** (`mapping(address => mapping(bytes4 => enum IRolesAuthority.CapabilityFlag))`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: RolesAuthority.setPublicCapability(address,bytes4,bool) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
  └─ [1] 🔒 MODIFIER: Auth.requiresAuth() (NodeID: 1)
      💬 Args: [no args]
    └─ [2] ⚙️ FUNCTION: Auth.isAuthorized(address,bytes4) (NodeID: 2)
        💬 Args: [msg.sender, msg.sig]
        👁️  Def: internal
```

## Documentation

### Function Documentation

@notice Set a capability flag as public, meaning any account can call it. Or revoke this capability.
 @dev A capability cannot be made public if it has been burned.
