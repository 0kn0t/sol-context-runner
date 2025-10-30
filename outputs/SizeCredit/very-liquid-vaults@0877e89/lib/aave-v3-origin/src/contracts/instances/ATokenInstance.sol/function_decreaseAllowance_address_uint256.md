# Function: decreaseAllowance(address,uint256)

**Contract**: [lib/aave-v3-origin/src/contracts/instances/ATokenInstance.sol/contract_ATokenInstance.md]

## Metadata

- **Contract**: ATokenInstance
- **Signature**: `decreaseAllowance(address,uint256)`
- **Visibility**: external
- **Source Range**: 5692:226:35
- **Inherited From**: IncentivizedERC20

## Implementation

```solidity
///  @notice Decreases the allowance of spender to spend _msgSender() tokens
///  @param spender The user allowed to spend on behalf of _msgSender()
///  @param subtractedValue The amount being subtracted to the allowance
///  @return `true`
function decreaseAllowance(address spender, uint256 subtractedValue) virtual external returns (bool) {
    _approve(_msgSender(), spender, _allowances[_msgSender()][spender] - subtractedValue);
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

## State Variable Reads

- **_allowances** (`mapping(address => mapping(address => uint256))`)

## State Variable Writes

- **_allowances** (`mapping(address => mapping(address => uint256))`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: IncentivizedERC20.decreaseAllowance(address,uint256) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: external
  └─ [1] ⚙️ FUNCTION: IncentivizedERC20._approve(address,address,uint256) (NodeID: 1)
      💬 Args: [_msgSender(), spender, _allowances[_msgSender()][spender] - subtractedValue]
      👁️  Def: internal
    ├─ [2] ⚙️ FUNCTION: Context._msgSender() (NodeID: 2)
    │   💬 Args: [no args]
    │   👁️  Def: internal
    └─ [2] ⚙️ FUNCTION: Context._msgSender() (NodeID: 3)
        💬 Args: [no args]
        👁️  Def: internal
```

## Documentation

### Function Documentation

 @notice Decreases the allowance of spender to spend _msgSender() tokens
 @param spender The user allowed to spend on behalf of _msgSender()
 @param subtractedValue The amount being subtracted to the allowance
 @return `true`
