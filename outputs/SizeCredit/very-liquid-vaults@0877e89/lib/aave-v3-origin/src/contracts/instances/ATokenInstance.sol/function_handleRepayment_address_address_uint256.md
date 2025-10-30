# Function: handleRepayment(address,address,uint256)

**Contract**: [lib/aave-v3-origin/src/contracts/instances/ATokenInstance.sol/contract_ATokenInstance.md]

## Metadata

- **Contract**: ATokenInstance
- **Signature**: `handleRepayment(address,address,uint256)`
- **Visibility**: external
- **Source Range**: 4325:163:31
- **Inherited From**: AToken

## Implementation

```solidity
/// @inheritdoc IAToken
function handleRepayment(address user, address onBehalfOf, uint256 amount) virtual override external onlyPool() {}
```

## Related Implementations

### onlyPool()

- **Kind**: modifier
- **Source**: 1449:104:35
- **Link**: `lib/aave-v3-origin/src/contracts/protocol/tokenization/base/IncentivizedERC20.sol:IncentivizedERC20:onlyPool()`

```solidity
///  @dev Only pool can call functions marked by this modifier.
modifier onlyPool() {
    require(_msgSender() == address(POOL), Errors.CALLER_MUST_BE_POOL);
    _;
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

- **POOL** (`contract IPool`) [lib/aave-v3-origin/src/contracts/interfaces/IPool.sol/interface_IPool.md]

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: AToken.handleRepayment(address,address,uint256) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: external
  └─ [1] 🔒 MODIFIER: IncentivizedERC20.onlyPool() (NodeID: 1)
      💬 Args: [no args]
    └─ [2] ⚙️ FUNCTION: Context._msgSender() (NodeID: 2)
        💬 Args: [no args]
        👁️  Def: internal
```

## Documentation

### Function Documentation

@inheritdoc IAToken

### Interface Documentation

 @notice Handles the underlying received by the aToken after the transfer has been completed.
 @dev The default implementation is empty as with standard ERC20 tokens, nothing needs to be done after the
 transfer is concluded. However in the future there may be aTokens that allow for example to stake the underlying
 to receive LM rewards. In that case, `handleRepayment()` would perform the staking of the underlying asset.
 @param user The user executing the repayment
 @param onBehalfOf The address of the user who will get his debt reduced/removed
 @param amount The amount getting repaid
