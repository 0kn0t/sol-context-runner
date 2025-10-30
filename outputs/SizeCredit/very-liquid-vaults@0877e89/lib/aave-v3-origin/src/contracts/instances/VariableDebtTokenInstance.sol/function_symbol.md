# Function: symbol()

**Contract**: [lib/aave-v3-origin/src/contracts/instances/VariableDebtTokenInstance.sol/contract_VariableDebtTokenInstance.md]

## Metadata

- **Contract**: VariableDebtTokenInstance
- **Signature**: `symbol()`
- **Visibility**: external
- **Source Range**: 2985:90:35
- **Inherited From**: IncentivizedERC20

## Implementation

```solidity
/// @inheritdoc IERC20Detailed
function symbol() override external view returns (string memory) {
    return _symbol;
}
```

## State Variable Reads

- **_symbol** (`string`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: IncentivizedERC20.symbol() (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: external
```

## Documentation

### Function Documentation

@inheritdoc IERC20Detailed
