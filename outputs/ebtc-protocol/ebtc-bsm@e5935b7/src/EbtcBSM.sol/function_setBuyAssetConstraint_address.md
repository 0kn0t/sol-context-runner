# Function: setBuyAssetConstraint(address)

**Contract**: [src/EbtcBSM.sol/contract_EbtcBSM.md]

## Metadata

- **Contract**: EbtcBSM
- **Signature**: `setBuyAssetConstraint(address)`
- **Visibility**: external
- **Source Range**: 18505:328:40

## Implementation

```solidity
/// @notice Updates the buy asset constraint address
///  @dev Can only be called by authorized users
///  @param _newBuyAssetConstraint New address for the buy asset constraint
function setBuyAssetConstraint(address _newBuyAssetConstraint) external requiresAuth() {
    require(_newBuyAssetConstraint != address(0), InvalidAddress());
    emit IConstraint.ConstraintUpdated(address(buyAssetConstraint), _newBuyAssetConstraint);
    buyAssetConstraint = IConstraint(_newBuyAssetConstraint);
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

- **buyAssetConstraint** (`contract IConstraint`) [src/Dependencies/IConstraint.sol/interface_IConstraint.md]
- **_authority** (`contract Authority`) [src/Dependencies/Authority.sol/interface_Authority.md]

## State Variable Writes

- **buyAssetConstraint** (`contract IConstraint`) [src/Dependencies/IConstraint.sol/interface_IConstraint.md]

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: EbtcBSM.setBuyAssetConstraint(address) (NodeID: 0)
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

@notice Updates the buy asset constraint address
 @dev Can only be called by authorized users
 @param _newBuyAssetConstraint New address for the buy asset constraint
