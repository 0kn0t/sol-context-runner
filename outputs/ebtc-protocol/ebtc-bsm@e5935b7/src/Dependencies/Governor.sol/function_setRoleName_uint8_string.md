# Function: setRoleName(uint8,string)

**Contract**: [src/Dependencies/Governor.sol/contract_Governor.md]

## Metadata

- **Contract**: Governor
- **Signature**: `setRoleName(uint8,string)`
- **Visibility**: external
- **Source Range**: 6637:165:28

## Implementation

```solidity
/// @notice Sets the name for a specific role ID for better readability
///  @dev This function requires authorization
///  @param role The role ID
///  @param roleName The name to assign to the role
function setRoleName(uint8 role, string memory roleName) external requiresAuth() {
    roleNames[role] = roleName;
    emit RoleNameSet(role, roleName);
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

- **authority** (`contract Authority`) [src/Dependencies/Authority.sol/interface_Authority.md]
- **owner** (`address`)

## State Variable Writes

- **roleNames** (`mapping(uint8 => string)`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: Governor.setRoleName(uint8,string) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: external
  └─ [1] 🔒 MODIFIER: Auth.requiresAuth() (NodeID: 1)
      💬 Args: [no args]
    └─ [2] ⚙️ FUNCTION: Auth.isAuthorized(address,bytes4) (NodeID: 2)
        💬 Args: [msg.sender, msg.sig]
        👁️  Def: internal
```

## Documentation

### Function Documentation

@notice Sets the name for a specific role ID for better readability
 @dev This function requires authorization
 @param role The role ID
 @param roleName The name to assign to the role
