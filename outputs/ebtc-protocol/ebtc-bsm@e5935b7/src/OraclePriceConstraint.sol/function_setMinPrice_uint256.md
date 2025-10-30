# Function: setMinPrice(uint256)

**Contract**: [src/OraclePriceConstraint.sol/contract_OraclePriceConstraint.md]

## Metadata

- **Contract**: OraclePriceConstraint
- **Signature**: `setMinPrice(uint256)`
- **Visibility**: external
- **Source Range**: 4369:222:41

## Implementation

```solidity
/// @notice Updates the minimum price threshold for minting
///  @dev Can only be called by authorized users
///  @param _minPriceBPS The new minimum price, in basis points
function setMinPrice(uint256 _minPriceBPS) external requiresAuth() {
    require(_minPriceBPS <= BPS, InvalidMinPrice());
    emit MinPriceUpdated(minPriceBPS, _minPriceBPS);
    minPriceBPS = _minPriceBPS;
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

- **BPS** (`uint256`)
- **minPriceBPS** (`uint256`)
- **_authority** (`contract Authority`) [src/Dependencies/Authority.sol/interface_Authority.md]

## State Variable Writes

- **minPriceBPS** (`uint256`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: OraclePriceConstraint.setMinPrice(uint256) (NodeID: 0)
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

@notice Updates the minimum price threshold for minting
 @dev Can only be called by authorized users
 @param _minPriceBPS The new minimum price, in basis points
