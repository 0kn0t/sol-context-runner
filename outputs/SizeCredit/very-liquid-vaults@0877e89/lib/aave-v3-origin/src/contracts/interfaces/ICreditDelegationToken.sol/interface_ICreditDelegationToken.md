# Interface: ICreditDelegationToken

## Metadata

- **Name**: ICreditDelegationToken
- **Type**: Interface
- **Path**: lib/aave-v3-origin/src/contracts/interfaces/ICreditDelegationToken.sol
- **Documentation**:  @title ICreditDelegationToken
   @author Aave
   @notice Defines the basic interface for a token supporting credit delegation.

## Events

### BorrowAllowanceDelegated

```solidity
///  @dev Emitted on `approveDelegation` and `borrowAllowance
///  @param fromUser The address of the delegator
///  @param toUser The address of the delegatee
///  @param asset The address of the delegated asset
///  @param amount The amount being delegated
event BorrowAllowanceDelegated(address indexed fromUser, address indexed toUser, address indexed asset, uint256 amount);
```

## Public/External Functions

### approveDelegation(address,uint256)

- **Signature**: `approveDelegation(address,uint256)`
- **Visibility**: external
- **Source Range**: 1008:71:15

**Signature:**
```solidity
///  @notice Delegates borrowing power to a user on the specific debt token.
///  Delegation will still respect the liquidation constraints (even if delegated, a
///  delegatee cannot force a delegator HF to go below 1)
///  @param delegatee The address receiving the delegated borrowing power
///  @param amount The maximum amount being delegated.
function approveDelegation(address delegatee, uint256 amount) external;;
```

### borrowAllowance(address,address)

- **Signature**: `borrowAllowance(address,address)`
- **Visibility**: external
- **Source Range**: 1295:91:15

**Signature:**
```solidity
///  @notice Returns the borrow allowance of the user
///  @param fromUser The user to giving allowance
///  @param toUser The user to give allowance to
///  @return The current allowance of `toUser`
function borrowAllowance(address fromUser, address toUser) external view returns (uint256);;
```

### delegationWithSig(address,address,uint256,uint256,uint8,bytes32,bytes32)

- **Signature**: `delegationWithSig(address,address,uint256,uint256,uint8,bytes32,bytes32)`
- **Visibility**: external
- **Source Range**: 1842:170:15

**Signature:**
```solidity
///  @notice Delegates borrowing power to a user on the specific debt token via ERC712 signature
///  @param delegator The delegator of the credit
///  @param delegatee The delegatee that can use the credit
///  @param value The amount to be delegated
///  @param deadline The deadline timestamp, type(uint256).max for max deadline
///  @param v The V signature param
///  @param s The S signature param
///  @param r The R signature param
function delegationWithSig(address delegator, address delegatee, uint256 value, uint256 deadline, uint8 v, bytes32 r, bytes32 s) external;;
```
