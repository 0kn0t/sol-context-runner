# Function: transferOwnership(address)

**Contract**: [src/Dependencies/Governor.sol/contract_Governor.md]

## Metadata

- **Contract**: Governor
- **Signature**: `transferOwnership(address)`
- **Visibility**: public
- **Source Range**: 2036:164:24
- **Inherited From**: Auth

## Implementation

```solidity
function transferOwnership(address newOwner) virtual public requiresAuth() {
    owner = newOwner;
    emit OwnershipTransferred(msg.sender, newOwner);
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

- **owner** (`address`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: Auth.transferOwnership(address) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
  └─ [1] 🔒 MODIFIER: Auth.requiresAuth() (NodeID: 1)
      💬 Args: [no args]
    └─ [2] ⚙️ FUNCTION: Auth.isAuthorized(address,bytes4) (NodeID: 2)
        💬 Args: [msg.sender, msg.sig]
        👁️  Def: internal
```
