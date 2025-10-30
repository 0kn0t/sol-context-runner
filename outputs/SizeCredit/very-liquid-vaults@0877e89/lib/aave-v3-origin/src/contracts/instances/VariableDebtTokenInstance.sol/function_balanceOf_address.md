# Function: balanceOf(address)

**Contract**: [lib/aave-v3-origin/src/contracts/instances/VariableDebtTokenInstance.sol/contract_VariableDebtTokenInstance.md]

## Metadata

- **Contract**: VariableDebtTokenInstance
- **Signature**: `balanceOf(address)`
- **Visibility**: public
- **Source Range**: 3356:128:35
- **Inherited From**: IncentivizedERC20

## Implementation

```solidity
/// @inheritdoc IERC20
function balanceOf(address account) virtual override public view returns (uint256) {
    return _userState[account].balance;
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

 @dev Returns the amount of tokens owned by `account`.
