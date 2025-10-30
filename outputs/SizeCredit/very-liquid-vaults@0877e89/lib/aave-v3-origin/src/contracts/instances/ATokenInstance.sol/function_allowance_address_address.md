# Function: allowance(address,address)

**Contract**: [lib/aave-v3-origin/src/contracts/instances/ATokenInstance.sol/contract_ATokenInstance.md]

## Metadata

- **Contract**: ATokenInstance
- **Signature**: `allowance(address,address)`
- **Visibility**: external
- **Source Range**: 4282:157:35
- **Inherited From**: IncentivizedERC20

## Implementation

```solidity
/// @inheritdoc IERC20
function allowance(address owner, address spender) virtual override external view returns (uint256) {
    return _allowances[owner][spender];
}
```

## State Variable Reads

- **_allowances** (`mapping(address => mapping(address => uint256))`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: IncentivizedERC20.allowance(address,address) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: external
```

## Documentation

### Function Documentation

@inheritdoc IERC20

### Interface Documentation

 @dev Returns the remaining number of tokens that `spender` will be
 allowed to spend on behalf of `owner` through {transferFrom}. This is
 zero by default.
 This value changes when {approve} or {transferFrom} are called.
