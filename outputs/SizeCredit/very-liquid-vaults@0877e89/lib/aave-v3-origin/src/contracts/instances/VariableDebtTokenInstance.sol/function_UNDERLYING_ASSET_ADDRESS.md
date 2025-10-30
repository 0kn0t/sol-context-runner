# Function: UNDERLYING_ASSET_ADDRESS()

**Contract**: [lib/aave-v3-origin/src/contracts/instances/VariableDebtTokenInstance.sol/contract_VariableDebtTokenInstance.md]

## Metadata

- **Contract**: VariableDebtTokenInstance
- **Signature**: `UNDERLYING_ASSET_ADDRESS()`
- **Visibility**: external
- **Source Range**: 4209:111:32
- **Inherited From**: VariableDebtToken

## Implementation

```solidity
/// @inheritdoc IVariableDebtToken
function UNDERLYING_ASSET_ADDRESS() override external view returns (address) {
    return _underlyingAsset;
}
```

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: VariableDebtToken.UNDERLYING_ASSET_ADDRESS() (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: external
```

## Documentation

### Function Documentation

@inheritdoc IVariableDebtToken

### Interface Documentation

 @notice Returns the address of the underlying asset of this debtToken (E.g. WETH for variableDebtWETH)
 @return The address of the underlying asset
