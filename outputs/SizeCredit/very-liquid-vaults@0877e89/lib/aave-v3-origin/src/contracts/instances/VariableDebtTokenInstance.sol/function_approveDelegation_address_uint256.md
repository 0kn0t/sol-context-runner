# Function: approveDelegation(address,uint256)

**Contract**: [lib/aave-v3-origin/src/contracts/instances/VariableDebtTokenInstance.sol/contract_VariableDebtTokenInstance.md]

## Metadata

- **Contract**: VariableDebtTokenInstance
- **Signature**: `approveDelegation(address,uint256)`
- **Visibility**: external
- **Source Range**: 1211:142:33
- **Inherited From**: DebtTokenBase

## Implementation

```solidity
/// @inheritdoc ICreditDelegationToken
function approveDelegation(address delegatee, uint256 amount) override external {
    _approveDelegation(_msgSender(), delegatee, amount);
}
```

## Related Implementations

### _approveDelegation(address,address,uint256)

- **Kind**: internal
- **Source**: 2723:233:33
- **Link**: `lib/aave-v3-origin/src/contracts/protocol/tokenization/base/DebtTokenBase.sol:DebtTokenBase:_approveDelegation(address,address,uint256)`

```solidity
///  @notice Updates the borrow allowance of a user on the specific debt token.
///  @param delegator The address delegating the borrowing power
///  @param delegatee The address receiving the delegated borrowing power
///  @param amount The allowance amount being delegated.
function _approveDelegation(address delegator, address delegatee, uint256 amount) internal {
    _borrowAllowances[delegator][delegatee] = amount;
    emit BorrowAllowanceDelegated(delegator, delegatee, _underlyingAsset, amount);
}
```

### _msgSender()

- **Kind**: internal
- **Source**: 588:107:2
- **Link**: `lib/aave-v3-origin/src/contracts/dependencies/openzeppelin/contracts/Context.sol:Context:_msgSender()`

```solidity
function _msgSender() virtual internal view returns (address payable) {
    return payable(msg.sender);
}
```

## State Variable Reads

- **_underlyingAsset** (`address`)

## State Variable Writes

- **_borrowAllowances** (`mapping(address => mapping(address => uint256))`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: DebtTokenBase.approveDelegation(address,uint256) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: external
  └─ [1] ⚙️ FUNCTION: DebtTokenBase._approveDelegation(address,address,uint256) (NodeID: 1)
      💬 Args: [_msgSender(), delegatee, amount]
      👁️  Def: internal
    └─ [2] ⚙️ FUNCTION: Context._msgSender() (NodeID: 2)
        💬 Args: [no args]
        👁️  Def: internal
```

## Documentation

### Function Documentation

@inheritdoc ICreditDelegationToken

### Interface Documentation

 @notice Delegates borrowing power to a user on the specific debt token.
 Delegation will still respect the liquidation constraints (even if delegated, a
 delegatee cannot force a delegator HF to go below 1)
 @param delegatee The address receiving the delegated borrowing power
 @param amount The maximum amount being delegated.
