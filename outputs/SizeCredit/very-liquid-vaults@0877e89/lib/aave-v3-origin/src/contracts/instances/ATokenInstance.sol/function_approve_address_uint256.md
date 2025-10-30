# Function: approve(address,uint256)

**Contract**: [lib/aave-v3-origin/src/contracts/instances/ATokenInstance.sol/contract_ATokenInstance.md]

## Metadata

- **Contract**: ATokenInstance
- **Signature**: `approve(address,uint256)`
- **Visibility**: external
- **Source Range**: 4468:158:35
- **Inherited From**: IncentivizedERC20

## Implementation

```solidity
/// @inheritdoc IERC20
function approve(address spender, uint256 amount) virtual override external returns (bool) {
    _approve(_msgSender(), spender, amount);
    return true;
}
```

## Related Implementations

### _approve(address,address,uint256)

- **Kind**: internal
- **Source**: 7169:173:35
- **Link**: `lib/aave-v3-origin/src/contracts/protocol/tokenization/base/IncentivizedERC20.sol:IncentivizedERC20:_approve(address,address,uint256)`

```solidity
///  @notice Approve `spender` to use `amount` of `owner`s balance
///  @param owner The address owning the tokens
///  @param spender The address approved for spending
///  @param amount The amount of tokens to approve spending of
function _approve(address owner, address spender, uint256 amount) virtual internal {
    _allowances[owner][spender] = amount;
    emit Approval(owner, spender, amount);
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

## State Variable Writes

- **_allowances** (`mapping(address => mapping(address => uint256))`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: IncentivizedERC20.approve(address,uint256) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: external
  └─ [1] ⚙️ FUNCTION: IncentivizedERC20._approve(address,address,uint256) (NodeID: 1)
      💬 Args: [_msgSender(), spender, amount]
      👁️  Def: internal
    └─ [2] ⚙️ FUNCTION: Context._msgSender() (NodeID: 2)
        💬 Args: [no args]
        👁️  Def: internal
```

## Documentation

### Function Documentation

@inheritdoc IERC20

### Interface Documentation

 @dev Sets `amount` as the allowance of `spender` over the caller's tokens.
 Returns a boolean value indicating whether the operation succeeded.
 IMPORTANT: Beware that changing an allowance with this method brings the risk
 that someone may use both the old and the new allowance by unfortunate
 transaction ordering. One possible solution to mitigate this race
 condition is to first reduce the spender's allowance to 0 and set the
 desired value afterwards:
 https://github.com/ethereum/EIPs/issues/20#issuecomment-263524729
 Emits an {Approval} event.
