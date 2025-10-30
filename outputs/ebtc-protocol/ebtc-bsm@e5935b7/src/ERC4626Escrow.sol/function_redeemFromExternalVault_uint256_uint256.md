# Function: redeemFromExternalVault(uint256,uint256)

**Contract**: [src/ERC4626Escrow.sol/contract_ERC4626Escrow.md]

## Metadata

- **Contract**: ERC4626Escrow
- **Signature**: `redeemFromExternalVault(uint256,uint256)`
- **Visibility**: external
- **Source Range**: 8603:303:39

## Implementation

```solidity
/// @notice Redeems shares from the external vault
///  @dev Can only be called by authorized users
///  @param sharesToRedeem The number of shares to redeem
///  @param minAssets The minimum acceptable assets to receive for the redeemed shares
function redeemFromExternalVault(uint256 sharesToRedeem, uint256 minAssets) external requiresAuth() {
    uint256 assets = EXTERNAL_VAULT.redeem(sharesToRedeem, address(this), address(this));
    if (assets < minAssets) {
        revert TooFewAssetsReceived(minAssets, assets);
    }
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

## External Calls

- **IERC4626::redeem(uint256,address,address)**

## State Variable Reads

- **EXTERNAL_VAULT** (`contract IERC4626`) [lib/openzeppelin-contracts/contracts/interfaces/IERC4626.sol/interface_IERC4626.md]
- **_authority** (`contract Authority`) [src/Dependencies/Authority.sol/interface_Authority.md]

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: ERC4626Escrow.redeemFromExternalVault(uint256,uint256) (NodeID: 0)
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

@notice Redeems shares from the external vault
 @dev Can only be called by authorized users
 @param sharesToRedeem The number of shares to redeem
 @param minAssets The minimum acceptable assets to receive for the redeemed shares
