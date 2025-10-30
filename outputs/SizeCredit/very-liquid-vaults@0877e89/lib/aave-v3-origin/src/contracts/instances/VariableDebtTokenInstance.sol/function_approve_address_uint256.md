# Function: approve(address,uint256)

**Contract**: [lib/aave-v3-origin/src/contracts/instances/VariableDebtTokenInstance.sol/contract_VariableDebtTokenInstance.md]

## Metadata

- **Contract**: VariableDebtTokenInstance
- **Signature**: `approve(address,uint256)`
- **Visibility**: external
- **Source Range**: 4468:158:35
- **Inherited From**: IncentivizedERC20

## Implementation

```solidity
/// @inheritdoc IERC20
function approve(address spender, uint256 amount) virtual override external returns (bool) {
    _approve(_msgSender(), spender, amount);
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

 @dev Sets `amount` as the allowance of `spender` over the caller's tokens.
 Returns a boolean value indicating whether the operation succeeded.
 IMPORTANT: Beware that changing an allowance with this method brings the risk
 that someone may use both the old and the new allowance by unfortunate
 transaction ordering. One possible solution to mitigate this race
 condition is to first reduce the spender's allowance to 0 and set the
 desired value afterwards:
 https://github.com/ethereum/EIPs/issues/20#issuecomment-263524729
 Emits an {Approval} event.
