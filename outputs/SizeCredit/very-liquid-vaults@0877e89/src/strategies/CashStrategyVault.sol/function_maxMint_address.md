# Function: maxMint(address)

**Contract**: [src/strategies/CashStrategyVault.sol/contract_CashStrategyVault.md]

## Metadata

- **Contract**: CashStrategyVault
- **Signature**: `maxMint(address)`
- **Visibility**: public
- **Source Range**: 7817:105:60
- **Inherited From**: ERC4626Upgradeable

## Implementation

```solidity
/// @dev See {IERC4626-maxMint}. 
function maxMint(address) virtual public view returns (uint256) {
    return type(uint256).max;
}
```

## Call Tree

```
No call tree available
```

## Documentation

### Function Documentation

@dev See {IERC4626-maxMint}. 

### Interface Documentation

 @dev Returns the maximum amount of the Vault shares that can be minted for the receiver, through a mint call.
 - MUST return a limited value if receiver is subject to some mint limit.
 - MUST return 2 ** 256 - 1 if there is no limit on the maximum amount of shares that may be minted.
 - MUST NOT revert.
