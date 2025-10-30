# Function: setFeeToBuy(uint256)

**Contract**: [src/EbtcBSM.sol/contract_EbtcBSM.md]

## Metadata

- **Contract**: EbtcBSM
- **Signature**: `setFeeToBuy(uint256)`
- **Visibility**: external
- **Source Range**: 16967:221:40

## Implementation

```solidity
/// @notice Sets the fee for buying eBTC
///  @dev Can only be called by authorized users
///  @param _feeToBuyBPS Fee in basis points
function setFeeToBuy(uint256 _feeToBuyBPS) external requiresAuth() {
    require(_feeToBuyBPS <= MAX_FEE, InvalidFee());
    emit FeeToBuyUpdated(feeToBuyBPS, _feeToBuyBPS);
    feeToBuyBPS = _feeToBuyBPS;
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

- **MAX_FEE** (`uint256`)
- **feeToBuyBPS** (`uint256`)
- **_authority** (`contract Authority`) [src/Dependencies/Authority.sol/interface_Authority.md]

## State Variable Writes

- **feeToBuyBPS** (`uint256`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: EbtcBSM.setFeeToBuy(uint256) (NodeID: 0)
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

@notice Sets the fee for buying eBTC
 @dev Can only be called by authorized users
 @param _feeToBuyBPS Fee in basis points
