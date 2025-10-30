# Function: burnCapability(address,bytes4)

**Contract**: [src/Dependencies/Governor.sol/contract_Governor.md]

## Metadata

- **Contract**: Governor
- **Signature**: `burnCapability(address,bytes4)`
- **Visibility**: public
- **Source Range**: 5349:367:37
- **Inherited From**: RolesAuthority

## Implementation

```solidity
/// @notice Permanently burns a capability for a target.
function burnCapability(address target, bytes4 functionSig) virtual public requiresAuth() {
    require(capabilityFlag[target][functionSig] != CapabilityFlag.Burned, "RolesAuthority: Capability Burned");
    capabilityFlag[target][functionSig] = CapabilityFlag.Burned;
    emit CapabilityBurned(target, functionSig);
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
┌─ [0] ⚙️ FUNCTION: RolesAuthority.burnCapability(address,bytes4) (NodeID: 0)
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

@notice Permanently burns a capability for a target.
