# Function: transfer(address,uint256)

**Contract**: [lib/aave-v3-origin/src/contracts/instances/VariableDebtTokenInstance.sol/contract_VariableDebtTokenInstance.md]

## Metadata

- **Contract**: VariableDebtTokenInstance
- **Signature**: `transfer(address,uint256)`
- **Visibility**: external
- **Source Range**: 4040:213:35
- **Inherited From**: IncentivizedERC20

## Implementation

```solidity
/// @inheritdoc IERC20
function transfer(address recipient, uint256 amount) virtual override external returns (bool) {
    uint128 castAmount = amount.toUint128();
    _transfer(_msgSender(), recipient, castAmount);
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

 @dev Moves `amount` tokens from the caller's account to `recipient`.
 Returns a boolean value indicating whether the operation succeeded.
 Emits a {Transfer} event.
