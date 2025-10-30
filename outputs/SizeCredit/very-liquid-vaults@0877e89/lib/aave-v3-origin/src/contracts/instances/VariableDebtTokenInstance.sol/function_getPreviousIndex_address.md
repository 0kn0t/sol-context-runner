# Function: getPreviousIndex(address)

**Contract**: [lib/aave-v3-origin/src/contracts/instances/VariableDebtTokenInstance.sol/contract_VariableDebtTokenInstance.md]

## Metadata

- **Contract**: VariableDebtTokenInstance
- **Signature**: `getPreviousIndex(address)`
- **Visibility**: external
- **Source Range**: 1751:138:37
- **Inherited From**: ScaledBalanceTokenBase

## Implementation

```solidity
/// @inheritdoc IScaledBalanceToken
function getPreviousIndex(address user) virtual override external view returns (uint256) {
    return _userState[user].additionalData;
}
```

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: ScaledBalanceTokenBase.getPreviousIndex(address) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: external
```

## Documentation

### Function Documentation

@inheritdoc IScaledBalanceToken

### Interface Documentation

 @notice Returns last index interest was accrued to the user's balance
 @param user The address of the user
 @return The last index interest was accrued to the user's balance, expressed in ray
