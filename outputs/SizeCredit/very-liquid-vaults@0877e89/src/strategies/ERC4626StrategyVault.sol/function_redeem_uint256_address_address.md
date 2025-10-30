# Function: redeem(uint256,address,address)

**Contract**: [src/strategies/ERC4626StrategyVault.sol/contract_ERC4626StrategyVault.md]

## Metadata

- **Contract**: ERC4626StrategyVault
- **Signature**: `redeem(uint256,address,address)`
- **Visibility**: public
- **Source Range**: 10443:405:60
- **Inherited From**: ERC4626Upgradeable

## Implementation

```solidity
/// @dev See {IERC4626-redeem}. 
function redeem(uint256 shares, address receiver, address owner) virtual public returns (uint256) {
    uint256 maxShares = maxRedeem(owner);
    if (shares > maxShares) {
        revert ERC4626ExceededMaxRedeem(owner, shares, maxShares);
    }
    uint256 assets = previewRedeem(shares);
    _withdraw(_msgSender(), receiver, owner, assets, shares);
    return assets;
}
```

## Call Tree

```
No call tree available
```

## Documentation

### Function Documentation

@dev See {IERC4626-redeem}. 

### Interface Documentation

 @dev Burns exactly shares from owner and sends assets of underlying tokens to receiver.
 - MUST emit the Withdraw event.
 - MAY support an additional flow in which the underlying tokens are owned by the Vault contract before the
   redeem execution, and are accounted for during redeem.
 - MUST revert if all of shares cannot be redeemed (due to withdrawal limit being reached, slippage, the owner
   not having enough shares, etc).
 NOTE: some implementations will require pre-requesting to the Vault before a withdrawal may be performed.
 Those methods should be performed separately.
