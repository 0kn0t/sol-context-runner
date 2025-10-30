# Function: transferFrom(address,address,uint256)

**Contract**: [lib/aave-v3-origin/src/contracts/instances/VariableDebtTokenInstance.sol/contract_VariableDebtTokenInstance.md]

## Metadata

- **Contract**: VariableDebtTokenInstance
- **Signature**: `transferFrom(address,address,uint256)`
- **Visibility**: external
- **Source Range**: 4655:327:35
- **Inherited From**: IncentivizedERC20

## Implementation

```solidity
/// @inheritdoc IERC20
function transferFrom(address sender, address recipient, uint256 amount) virtual override external returns (bool) {
    uint128 castAmount = amount.toUint128();
    _approve(sender, _msgSender(), _allowances[sender][_msgSender()] - castAmount);
    _transfer(sender, recipient, castAmount);
    return true;
}
```

## Call Tree

```
No call tree available
```

## Documentation

### Function Documentation

@inheritdoc IERC20

### Interface Documentation

 @dev Moves `amount` tokens from `sender` to `recipient` using the
 allowance mechanism. `amount` is then deducted from the caller's
 allowance.
 Returns a boolean value indicating whether the operation succeeded.
 Emits a {Transfer} event.
