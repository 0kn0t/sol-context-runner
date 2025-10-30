# Function: decimals()

**Contract**: [lib/aave-v3-origin/src/contracts/instances/VariableDebtTokenInstance.sol/contract_VariableDebtTokenInstance.md]

## Metadata

- **Contract**: VariableDebtTokenInstance
- **Signature**: `decimals()`
- **Visibility**: external
- **Source Range**: 3112:86:35
- **Inherited From**: IncentivizedERC20

## Implementation

```solidity
/// @inheritdoc IERC20Detailed
function decimals() override external view returns (uint8) {
    return _decimals;
}
```

## State Variable Reads

- **_decimals** (`uint8`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: IncentivizedERC20.decimals() (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: external
```

## Documentation

### Function Documentation

@inheritdoc IERC20Detailed
