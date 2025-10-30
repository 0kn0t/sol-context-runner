# Function: convertToShares(uint256)

**Contract**: [src/strategies/AaveStrategyVault.sol/contract_AaveStrategyVault.md]

## Metadata

- **Contract**: AaveStrategyVault
- **Signature**: `convertToShares(uint256)`
- **Visibility**: public
- **Source Range**: 7264:148:60
- **Inherited From**: ERC4626Upgradeable

## Implementation

```solidity
/// @dev See {IERC4626-convertToShares}. 
function convertToShares(uint256 assets) virtual public view returns (uint256) {
    return _convertToShares(assets, Math.Rounding.Floor);
}
```

## Call Tree

```
No call tree available
```

## Documentation

### Function Documentation

@dev See {IERC4626-convertToShares}. 

### Interface Documentation

 @dev Returns the amount of shares that the Vault would exchange for the amount of assets provided, in an ideal
 scenario where all the conditions are met.
 - MUST NOT be inclusive of any fees that are charged against assets in the Vault.
 - MUST NOT show any variations depending on the caller.
 - MUST NOT reflect slippage or other on-chain conditions, when performing the actual exchange.
 - MUST NOT revert.
 NOTE: This calculation MAY NOT reflect the “per-user” price-per-share, and instead should reflect the
 “average-user’s” price-per-share, meaning what the average user should expect to see when exchanging to and
 from.
