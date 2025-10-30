# Function: mint(uint256,address)

**Contract**: [src/strategies/CashStrategyVault.sol/contract_CashStrategyVault.md]

## Metadata

- **Contract**: CashStrategyVault
- **Signature**: `mint(uint256,address)`
- **Visibility**: public
- **Source Range**: 9558:380:60
- **Inherited From**: ERC4626Upgradeable

## Implementation

```solidity
/// @dev See {IERC4626-mint}. 
function mint(uint256 shares, address receiver) virtual public returns (uint256) {
    uint256 maxShares = maxMint(receiver);
    if (shares > maxShares) {
        revert ERC4626ExceededMaxMint(receiver, shares, maxShares);
    }
    uint256 assets = previewMint(shares);
    _deposit(_msgSender(), receiver, assets, shares);
    return assets;
}
```

## Call Tree

```
No call tree available
```

## Documentation

### Function Documentation

@dev See {IERC4626-mint}. 

### Interface Documentation

 @dev Mints exactly shares Vault shares to receiver by depositing amount of underlying tokens.
 - MUST emit the Deposit event.
 - MAY support an additional flow in which the underlying tokens are owned by the Vault contract before the mint
   execution, and are accounted for during mint.
 - MUST revert if all of shares cannot be minted (due to deposit limit being reached, slippage, the user not
   approving enough underlying tokens to the Vault contract, etc).
 NOTE: most implementations will require pre-approval of the Vault with the Vault’s underlying asset token.
