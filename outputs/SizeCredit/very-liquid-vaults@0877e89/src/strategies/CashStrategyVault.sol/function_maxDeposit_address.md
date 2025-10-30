# Function: maxDeposit(address)

**Contract**: [src/strategies/CashStrategyVault.sol/contract_CashStrategyVault.md]

## Metadata

- **Contract**: CashStrategyVault
- **Signature**: `maxDeposit(address)`
- **Visibility**: public
- **Source Range**: 7663:108:60
- **Inherited From**: ERC4626Upgradeable

## Implementation

```solidity
/// @dev See {IERC4626-maxDeposit}. 
function maxDeposit(address) virtual public view returns (uint256) {
    return type(uint256).max;
}
```

## Call Tree

```
No call tree available
```

## Documentation

### Function Documentation

@dev See {IERC4626-maxDeposit}. 

### Interface Documentation

 @dev Returns the maximum amount of the underlying asset that can be deposited into the Vault for the receiver,
 through a deposit call.
 - MUST return a limited value if receiver is subject to some deposit limit.
 - MUST return 2 ** 256 - 1 if there is no limit on the maximum amount of assets that may be deposited.
 - MUST NOT revert.
