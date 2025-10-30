# Function: setRateLimitingConstraint(address)

**Contract**: [src/EbtcBSM.sol/contract_EbtcBSM.md]

## Metadata

- **Contract**: EbtcBSM
- **Signature**: `setRateLimitingConstraint(address)`
- **Visibility**: external
- **Source Range**: 17397:356:40

## Implementation

```solidity
/// @notice Updates the rate limiting constraint address
///  @dev Can only be called by authorized users
///  @param _newRateLimitingConstraint New address for the rate limiting constraint
function setRateLimitingConstraint(address _newRateLimitingConstraint) external requiresAuth() {
    require(_newRateLimitingConstraint != address(0), InvalidAddress());
    emit IConstraint.ConstraintUpdated(address(rateLimitingConstraint), _newRateLimitingConstraint);
    rateLimitingConstraint = IConstraint(_newRateLimitingConstraint);
}
```

## Related Implementations

### requiresAuth()

- **Kind**: modifier
- **Source**: 647:125:25
- **Link**: `src/Dependencies/AuthNoOwner.sol:AuthNoOwner:requiresAuth()`

```solidity
modifier requiresAuth() virtual {
    require(isAuthorized(msg.sender, msg.sig), "Auth: UNAUTHORIZED");
    _;
}
```

### isAuthorized(address,bytes4)

- **Kind**: internal
- **Source**: 981:524:25
- **Link**: `src/Dependencies/AuthNoOwner.sol:AuthNoOwner:isAuthorized(address,bytes4)`

```solidity
function isAuthorized(address user, bytes4 functionSig) virtual internal view returns (bool) {
    Authority auth = _authority;
    return ((address(auth) != address(0)) && auth.canCall(user, address(this), functionSig));
}
```

## State Variable Reads

- **rateLimitingConstraint** (`contract IConstraint`) [src/Dependencies/IConstraint.sol/interface_IConstraint.md]
- **_authority** (`contract Authority`) [src/Dependencies/Authority.sol/interface_Authority.md]

## State Variable Writes

- **rateLimitingConstraint** (`contract IConstraint`) [src/Dependencies/IConstraint.sol/interface_IConstraint.md]

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: EbtcBSM.setRateLimitingConstraint(address) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: external
  └─ [1] 🔒 MODIFIER: AuthNoOwner.requiresAuth() (NodeID: 1)
      💬 Args: [no args]
    └─ [2] ⚙️ FUNCTION: AuthNoOwner.isAuthorized(address,bytes4) (NodeID: 2)
        💬 Args: [msg.sender, msg.sig]
        👁️  Def: internal
```

## Documentation

### Function Documentation

@notice Updates the rate limiting constraint address
 @dev Can only be called by authorized users
 @param _newRateLimitingConstraint New address for the rate limiting constraint
