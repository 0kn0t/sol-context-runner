# Function: deposit(uint256,address)

**Contract**: [src/strategies/ERC4626StrategyVault.sol/contract_ERC4626StrategyVault.md]

## Metadata

- **Contract**: ERC4626StrategyVault
- **Signature**: `deposit(uint256,address)`
- **Visibility**: public
- **Source Range**: 9123:392:60
- **Inherited From**: ERC4626Upgradeable

## Implementation

```solidity
/// @dev See {IERC4626-deposit}. 
function deposit(uint256 assets, address receiver) virtual public returns (uint256) {
    uint256 maxAssets = maxDeposit(receiver);
    if (assets > maxAssets) {
        revert ERC4626ExceededMaxDeposit(receiver, assets, maxAssets);
    }
    uint256 shares = previewDeposit(assets);
    _deposit(_msgSender(), receiver, assets, shares);
    return shares;
}
```

## Call Tree

```
No call tree available
```

## Documentation

### Function Documentation

@dev See {IERC4626-deposit}. 

### Interface Documentation

 @dev Mints shares Vault shares to receiver by depositing exactly amount of underlying tokens.
 - MUST emit the Deposit event.
 - MAY support an additional flow in which the underlying tokens are owned by the Vault contract before the
   deposit execution, and are accounted for during deposit.
 - MUST revert if all of assets cannot be deposited (due to deposit limit being reached, slippage, the user not
   approving enough underlying tokens to the Vault contract, etc).
 NOTE: most implementations will require pre-approval of the Vault with the Vault’s underlying asset token.
