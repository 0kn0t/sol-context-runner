# Function: increaseAllowance(address,uint256)

**Contract**: [lib/aave-v3-origin/src/contracts/instances/VariableDebtTokenInstance.sol/contract_VariableDebtTokenInstance.md]

## Metadata

- **Contract**: VariableDebtTokenInstance
- **Signature**: `increaseAllowance(address,uint256)`
- **Visibility**: external
- **Source Range**: 5230:204:35
- **Inherited From**: IncentivizedERC20

## Implementation

```solidity
///  @notice Increases the allowance of spender to spend _msgSender() tokens
///  @param spender The user allowed to spend on behalf of _msgSender()
///  @param addedValue The amount being added to the allowance
///  @return `true`
function increaseAllowance(address spender, uint256 addedValue) virtual external returns (bool) {
    _approve(_msgSender(), spender, _allowances[_msgSender()][spender] + addedValue);
    return true;
}
```

## Call Tree

```
No call tree available
```

## Documentation

### Function Documentation

 @notice Increases the allowance of spender to spend _msgSender() tokens
 @param spender The user allowed to spend on behalf of _msgSender()
 @param addedValue The amount being added to the allowance
 @return `true`
