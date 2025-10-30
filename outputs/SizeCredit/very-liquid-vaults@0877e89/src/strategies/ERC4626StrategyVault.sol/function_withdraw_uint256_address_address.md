# Function: withdraw(uint256,address,address)

**Contract**: [src/strategies/ERC4626StrategyVault.sol/contract_ERC4626StrategyVault.md]

## Metadata

- **Contract**: ERC4626StrategyVault
- **Signature**: `withdraw(uint256,address,address)`
- **Visibility**: public
- **Source Range**: 9985:413:60
- **Inherited From**: ERC4626Upgradeable

## Implementation

```solidity
/// @dev See {IERC4626-withdraw}. 
function withdraw(uint256 assets, address receiver, address owner) virtual public returns (uint256) {
    uint256 maxAssets = maxWithdraw(owner);
    if (assets > maxAssets) {
        revert ERC4626ExceededMaxWithdraw(owner, assets, maxAssets);
    }
    uint256 shares = previewWithdraw(assets);
    _withdraw(_msgSender(), receiver, owner, assets, shares);
    return shares;
}
```

## Call Tree

```
No call tree available
```

## Documentation

### Function Documentation

@dev See {IERC4626-withdraw}. 

### Interface Documentation

 @dev Burns shares from owner and sends exactly assets of underlying tokens to receiver.
 - MUST emit the Withdraw event.
 - MAY support an additional flow in which the underlying tokens are owned by the Vault contract before the
   withdraw execution, and are accounted for during withdraw.
 - MUST revert if all of assets cannot be withdrawn (due to withdrawal limit being reached, slippage, the owner
   not having enough shares, etc).
 Note that some implementations will require pre-requesting to the Vault before a withdrawal may be performed.
 Those methods should be performed separately.
