# Function: maxWithdraw(address)

**Contract**: [src/strategies/CashStrategyVault.sol/contract_CashStrategyVault.md]

## Metadata

- **Contract**: CashStrategyVault
- **Signature**: `maxWithdraw(address)`
- **Visibility**: public
- **Source Range**: 7972:153:60
- **Inherited From**: ERC4626Upgradeable

## Implementation

```solidity
/// @dev See {IERC4626-maxWithdraw}. 
function maxWithdraw(address owner) virtual public view returns (uint256) {
    return _convertToAssets(balanceOf(owner), Math.Rounding.Floor);
}
```

## Call Tree

```
No call tree available
```

## Documentation

### Function Documentation

@dev See {IERC4626-maxWithdraw}. 

### Interface Documentation

 @dev Returns the maximum amount of the underlying asset that can be withdrawn from the owner balance in the
 Vault, through a withdraw call.
 - MUST return a limited value if owner is subject to some withdrawal limit or timelock.
 - MUST NOT revert.
