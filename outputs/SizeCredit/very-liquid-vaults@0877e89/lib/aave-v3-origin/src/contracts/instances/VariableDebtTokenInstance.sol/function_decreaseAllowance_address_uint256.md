# Function: decreaseAllowance(address,uint256)

**Contract**: [lib/aave-v3-origin/src/contracts/instances/VariableDebtTokenInstance.sol/contract_VariableDebtTokenInstance.md]

## Metadata

- **Contract**: VariableDebtTokenInstance
- **Signature**: `decreaseAllowance(address,uint256)`
- **Visibility**: external
- **Source Range**: 5692:226:35
- **Inherited From**: IncentivizedERC20

## Implementation

```solidity
///  @notice Decreases the allowance of spender to spend _msgSender() tokens
///  @param spender The user allowed to spend on behalf of _msgSender()
///  @param subtractedValue The amount being subtracted to the allowance
///  @return `true`
function decreaseAllowance(address spender, uint256 subtractedValue) virtual external returns (bool) {
    _approve(_msgSender(), spender, _allowances[_msgSender()][spender] - subtractedValue);
    return true;
}
```

## Call Tree

```
No call tree available
```

## Documentation

### Function Documentation

 @notice Decreases the allowance of spender to spend _msgSender() tokens
 @param spender The user allowed to spend on behalf of _msgSender()
 @param subtractedValue The amount being subtracted to the allowance
 @return `true`
