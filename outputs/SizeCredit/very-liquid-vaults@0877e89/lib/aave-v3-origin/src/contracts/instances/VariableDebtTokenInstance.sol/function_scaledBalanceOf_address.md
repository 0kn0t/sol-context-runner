# Function: scaledBalanceOf(address)

**Contract**: [lib/aave-v3-origin/src/contracts/instances/VariableDebtTokenInstance.sol/contract_VariableDebtTokenInstance.md]

## Metadata

- **Contract**: VariableDebtTokenInstance
- **Signature**: `scaledBalanceOf(address)`
- **Visibility**: external
- **Source Range**: 1220:119:37
- **Inherited From**: ScaledBalanceTokenBase

## Implementation

```solidity
/// @inheritdoc IScaledBalanceToken
function scaledBalanceOf(address user) override external view returns (uint256) {
    return super.balanceOf(user);
}
```

## Related Implementations

### balanceOf(address)

- **Kind**: internal
- **Source**: 3356:128:35
- **Link**: `lib/aave-v3-origin/src/contracts/protocol/tokenization/base/IncentivizedERC20.sol:IncentivizedERC20:balanceOf(address)`

```solidity
/// @inheritdoc IERC20
function balanceOf(address account) virtual override public view returns (uint256) {
    return _userState[account].balance;
}
```

## State Variable Reads

- **_userState** (`mapping(address => struct IncentivizedERC20.UserState)`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: ScaledBalanceTokenBase.scaledBalanceOf(address) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: external
  └─ [1] ⚙️ FUNCTION: IncentivizedERC20.balanceOf(address) (NodeID: 1)
      💬 Args: [user]
      👁️  Def: public
```

## Documentation

### Function Documentation

@inheritdoc IScaledBalanceToken

### Interface Documentation

 @notice Returns the scaled balance of the user.
 @dev The scaled balance is the sum of all the updated stored balance divided by the reserve's liquidity index
 at the moment of the update
 @param user The user whose balance is calculated
 @return The scaled balance of the user
