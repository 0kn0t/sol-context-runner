# Function: getScaledUserBalanceAndSupply(address)

**Contract**: [lib/aave-v3-origin/src/contracts/instances/ATokenInstance.sol/contract_ATokenInstance.md]

## Metadata

- **Contract**: ATokenInstance
- **Signature**: `getScaledUserBalanceAndSupply(address)`
- **Visibility**: external
- **Source Range**: 1381:173:37
- **Inherited From**: ScaledBalanceTokenBase

## Implementation

```solidity
/// @inheritdoc IScaledBalanceToken
function getScaledUserBalanceAndSupply(address user) override external view returns (uint256, uint256) {
    return (super.balanceOf(user), super.totalSupply());
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

- **_userState** (`mapping(address => struct IncentivizedERC20.UserState)`)
- **_totalSupply** (`uint256`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: ScaledBalanceTokenBase.getScaledUserBalanceAndSupply(address) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: external
  ├─ [1] ⚙️ FUNCTION: IncentivizedERC20.balanceOf(address) (NodeID: 1)
  │   💬 Args: [user]
  │   👁️  Def: public
  └─ [1] ⚙️ FUNCTION: IncentivizedERC20.totalSupply() (NodeID: 2)
      💬 Args: [no args]
      👁️  Def: public
```

## Documentation

### Function Documentation

@inheritdoc IScaledBalanceToken

### Interface Documentation

 @notice Returns the scaled balance of the user and the scaled total supply.
 @param user The address of the user
 @return The scaled balance of the user
 @return The scaled total supply
