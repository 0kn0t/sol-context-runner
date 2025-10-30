# Function: borrowAllowance(address,address)

**Contract**: [lib/aave-v3-origin/src/contracts/instances/VariableDebtTokenInstance.sol/contract_VariableDebtTokenInstance.md]

## Metadata

- **Contract**: VariableDebtTokenInstance
- **Signature**: `borrowAllowance(address,address)`
- **Visibility**: external
- **Source Range**: 2266:165:33
- **Inherited From**: DebtTokenBase

## Implementation

```solidity
/// @inheritdoc ICreditDelegationToken
function borrowAllowance(address fromUser, address toUser) override external view returns (uint256) {
    return _borrowAllowances[fromUser][toUser];
}
```

## State Variable Reads

- **_borrowAllowances** (`mapping(address => mapping(address => uint256))`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: DebtTokenBase.borrowAllowance(address,address) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: external
```

## Documentation

### Function Documentation

@inheritdoc ICreditDelegationToken

### Interface Documentation

 @notice Returns the borrow allowance of the user
 @param fromUser The user to giving allowance
 @param toUser The user to give allowance to
 @return The current allowance of `toUser`
