# Function: depositToExternalVault(uint256,uint256)

**Contract**: [src/ERC4626Escrow.sol/contract_ERC4626Escrow.md]

## Metadata

- **Contract**: ERC4626Escrow
- **Signature**: `depositToExternalVault(uint256,uint256)`
- **Visibility**: external
- **Source Range**: 7964:375:39

## Implementation

```solidity
/// @notice Deposits assets into the external vault
///  @dev Can only be called by authorized users
///  @dev Can cause losses if the admin is not careful
///  @dev Assumes: Slippage check is safe
///  @param assetsToDeposit The amount of assets to deposit
///  @param minShares The minimum acceptable shares to receive for the deposited assets
function depositToExternalVault(uint256 assetsToDeposit, uint256 minShares) external requiresAuth() {
    ASSET_TOKEN.safeIncreaseAllowance(address(EXTERNAL_VAULT), assetsToDeposit);
    uint256 shares = EXTERNAL_VAULT.deposit(assetsToDeposit, address(this));
    if (shares < minShares) {
        revert TooFewSharesReceived(minShares, shares);
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

- **IERC20::safeIncreaseAllowance(contract IERC20,address,uint256)**
- **IERC4626::deposit(uint256,address)**

## State Variable Reads

- **EXTERNAL_VAULT** (`contract IERC4626`) [lib/openzeppelin-contracts/contracts/interfaces/IERC4626.sol/interface_IERC4626.md]
- **_authority** (`contract Authority`) [src/Dependencies/Authority.sol/interface_Authority.md]

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: ERC4626Escrow.depositToExternalVault(uint256,uint256) (NodeID: 0)
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

@notice Deposits assets into the external vault
 @dev Can only be called by authorized users
 @dev Can cause losses if the admin is not careful
 @dev Assumes: Slippage check is safe
 @param assetsToDeposit The amount of assets to deposit
 @param minShares The minimum acceptable shares to receive for the deposited assets
