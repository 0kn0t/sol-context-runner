# Function: convertToAssets(uint256)

**Contract**: [src/strategies/AaveStrategyVault.sol/contract_AaveStrategyVault.md]

## Metadata

- **Contract**: AaveStrategyVault
- **Signature**: `convertToAssets(uint256)`
- **Visibility**: public
- **Source Range**: 7466:148:60
- **Inherited From**: ERC4626Upgradeable

## Implementation

```solidity
/// @dev See {IERC4626-convertToAssets}. 
function convertToAssets(uint256 shares) virtual public view returns (uint256) {
    return _convertToAssets(shares, Math.Rounding.Floor);
}
```

## Call Tree

```
No call tree available
```

## Documentation

### Function Documentation

@dev See {IERC4626-convertToAssets}. 

### Interface Documentation

 @dev Returns the amount of assets that the Vault would exchange for the amount of shares provided, in an ideal
 scenario where all the conditions are met.
 - MUST NOT be inclusive of any fees that are charged against assets in the Vault.
 - MUST NOT show any variations depending on the caller.
 - MUST NOT reflect slippage or other on-chain conditions, when performing the actual exchange.
 - MUST NOT revert.
 NOTE: This calculation MAY NOT reflect the “per-user” price-per-share, and instead should reflect the
 “average-user’s” price-per-share, meaning what the average user should expect to see when exchanging to and
 from.
