# Function: maxRedeem(address)

**Contract**: [src/strategies/CashStrategyVault.sol/contract_CashStrategyVault.md]

## Metadata

- **Contract**: CashStrategyVault
- **Signature**: `maxRedeem(address)`
- **Visibility**: public
- **Source Range**: 8173:112:60
- **Inherited From**: ERC4626Upgradeable

## Implementation

```solidity
/// @dev See {IERC4626-maxRedeem}. 
function maxRedeem(address owner) virtual public view returns (uint256) {
    return balanceOf(owner);
}
```

## Call Tree

```
No call tree available
```

## Documentation

### Function Documentation

@dev See {IERC4626-maxRedeem}. 

### Interface Documentation

 @dev Returns the maximum amount of Vault shares that can be redeemed from the owner balance in the Vault,
 through a redeem call.
 - MUST return a limited value if owner is subject to some withdrawal limit or timelock.
 - MUST return balanceOf(owner) if owner is not subject to any withdrawal limit or timelock.
 - MUST NOT revert.
