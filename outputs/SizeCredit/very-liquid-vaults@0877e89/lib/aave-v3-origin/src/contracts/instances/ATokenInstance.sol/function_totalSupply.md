# Function: totalSupply()

**Contract**: [lib/aave-v3-origin/src/contracts/instances/ATokenInstance.sol/contract_ATokenInstance.md]

## Metadata

- **Contract**: ATokenInstance
- **Signature**: `totalSupply()`
- **Visibility**: public
- **Source Range**: 3227:100:35
- **Inherited From**: IncentivizedERC20

## Implementation

```solidity
/// @inheritdoc IERC20
function totalSupply() virtual override public view returns (uint256) {
    return _totalSupply;
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

 @dev Returns the amount of tokens in existence.
