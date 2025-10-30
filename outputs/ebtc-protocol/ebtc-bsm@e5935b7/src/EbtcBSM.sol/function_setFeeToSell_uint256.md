# Function: setFeeToSell(uint256)

**Contract**: [src/EbtcBSM.sol/contract_EbtcBSM.md]

## Metadata

- **Contract**: EbtcBSM
- **Signature**: `setFeeToSell(uint256)`
- **Visibility**: external
- **Source Range**: 16584:229:40

## Implementation

```solidity
/// @notice Sets the fee for selling eBTC
///  @dev Can only be called by authorized users
///  @param _feeToSellBPS Fee in basis points
function setFeeToSell(uint256 _feeToSellBPS) external requiresAuth() {
    require(_feeToSellBPS <= MAX_FEE, InvalidFee());
    emit FeeToSellUpdated(feeToSellBPS, _feeToSellBPS);
    feeToSellBPS = _feeToSellBPS;
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
- **feeToSellBPS** (`uint256`)
- **_authority** (`contract Authority`) [src/Dependencies/Authority.sol/interface_Authority.md]

## State Variable Writes

- **feeToSellBPS** (`uint256`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: EbtcBSM.setFeeToSell(uint256) (NodeID: 0)
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

@notice Sets the fee for selling eBTC
 @dev Can only be called by authorized users
 @param _feeToSellBPS Fee in basis points
