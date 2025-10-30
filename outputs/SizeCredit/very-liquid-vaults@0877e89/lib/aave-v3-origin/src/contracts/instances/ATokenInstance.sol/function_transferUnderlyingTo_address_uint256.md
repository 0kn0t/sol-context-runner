# Function: transferUnderlyingTo(address,uint256)

**Contract**: [lib/aave-v3-origin/src/contracts/instances/ATokenInstance.sol/contract_ATokenInstance.md]

## Metadata

- **Contract**: ATokenInstance
- **Signature**: `transferUnderlyingTo(address,uint256)`
- **Visibility**: external
- **Source Range**: 4134:161:31
- **Inherited From**: AToken

## Implementation

```solidity
/// @inheritdoc IAToken
function transferUnderlyingTo(address target, uint256 amount) virtual override external onlyPool() {
    IERC20(_underlyingAsset).safeTransfer(target, amount);
}
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

## External Calls

- **IERC20::safeTransfer(contract IERC20,address,uint256)**

## State Variable Reads

- **_underlyingAsset** (`address`)
- **POOL** (`contract IPool`) [lib/aave-v3-origin/src/contracts/interfaces/IPool.sol/interface_IPool.md]

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: AToken.transferUnderlyingTo(address,uint256) (NodeID: 0)
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

 @notice Transfers the underlying asset to `target`.
 @dev Used by the Pool to transfer assets in borrow(), withdraw() and flashLoan()
 @param target The recipient of the underlying
 @param amount The amount getting transferred
