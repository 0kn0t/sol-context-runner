# Function: scaledTotalSupply()

**Contract**: [lib/aave-v3-origin/src/contracts/instances/VariableDebtTokenInstance.sol/contract_VariableDebtTokenInstance.md]

## Metadata

- **Contract**: VariableDebtTokenInstance
- **Signature**: `scaledTotalSupply()`
- **Visibility**: public
- **Source Range**: 1596:113:37
- **Inherited From**: ScaledBalanceTokenBase

## Implementation

```solidity
/// @inheritdoc IScaledBalanceToken
function scaledTotalSupply() virtual override public view returns (uint256) {
    return super.totalSupply();
}
```

## Related Implementations

### totalSupply()

- **Kind**: internal
- **Source**: 3227:100:35
- **Link**: `lib/aave-v3-origin/src/contracts/protocol/tokenization/base/IncentivizedERC20.sol:IncentivizedERC20:totalSupply()`

```solidity
/// @inheritdoc IERC20
function totalSupply() virtual override public view returns (uint256) {
    return _totalSupply;
}
```

## State Variable Reads

- **_totalSupply** (`uint256`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: ScaledBalanceTokenBase.scaledTotalSupply() (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
  └─ [1] ⚙️ FUNCTION: IncentivizedERC20.totalSupply() (NodeID: 1)
      💬 Args: [no args]
      👁️  Def: public
```

## Documentation

### Function Documentation

@inheritdoc IScaledBalanceToken

### Interface Documentation

 @notice Returns the scaled total supply of the scaled balance token. Represents sum(debt/index)
 @return The scaled total supply
