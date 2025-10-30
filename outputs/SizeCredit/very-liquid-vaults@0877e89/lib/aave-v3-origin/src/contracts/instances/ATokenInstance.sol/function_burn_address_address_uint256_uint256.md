# Function: burn(address,address,uint256,uint256)

**Contract**: [lib/aave-v3-origin/src/contracts/instances/ATokenInstance.sol/contract_ATokenInstance.md]

## Metadata

- **Contract**: ATokenInstance
- **Signature**: `burn(address,address,uint256,uint256)`
- **Visibility**: external
- **Source Range**: 2361:339:31
- **Inherited From**: AToken

## Implementation

```solidity
/// @inheritdoc IAToken
function burn(address from, address receiverOfUnderlying, uint256 amount, uint256 index) virtual override external onlyPool() {
    _burnScaled(from, receiverOfUnderlying, amount, index);
    if (receiverOfUnderlying != address(this)) {
        IERC20(_underlyingAsset).safeTransfer(receiverOfUnderlying, amount);
    }
}
```

## Related Implementations

### _burnScaled(address,address,uint256,uint256)

- **Kind**: internal
- **Source**: 3507:888:37
- **Link**: `lib/aave-v3-origin/src/contracts/protocol/tokenization/base/ScaledBalanceTokenBase.sol:ScaledBalanceTokenBase:_burnScaled(address,address,uint256,uint256)`

```solidity
///  @notice Implements the basic logic to burn a scaled balance token.
///  @dev In some instances, a burn transaction will emit a mint event
///  if the amount to burn is less than the interest that the user accrued
///  @param user The user which debt is burnt
///  @param target The address that will receive the underlying, if any
///  @param amount The amount getting burned
///  @param index The variable debt index of the reserve
function _burnScaled(address user, address target, uint256 amount, uint256 index) internal {
    uint256 amountScaled = amount.rayDiv(index);
    require(amountScaled != 0, Errors.INVALID_BURN_AMOUNT);
    uint256 scaledBalance = super.balanceOf(user);
    uint256 balanceIncrease = scaledBalance.rayMul(index) - scaledBalance.rayMul(_userState[user].additionalData);
    _userState[user].additionalData = index.toUint128();
    _burn(user, amountScaled.toUint128());
    if (balanceIncrease > amount) {
        uint256 amountToMint = balanceIncrease - amount;
        emit Transfer(address(0), user, amountToMint);
        emit Mint(user, user, amountToMint, balanceIncrease, index);
    } else {
        uint256 amountToBurn = amount - balanceIncrease;
        emit Transfer(user, address(0), amountToBurn);
        emit Burn(user, target, amountToBurn, balanceIncrease, index);
    }
}
```

### rayDiv(uint256,uint256)

- **Kind**: internal
- **Source**: 2840:322:29
- **Link**: `lib/aave-v3-origin/src/contracts/protocol/libraries/math/WadRayMath.sol:WadRayMath:rayDiv(uint256,uint256)`

```solidity
///  @notice Divides two ray, rounding half up to the nearest ray
///  @dev assembly optimized for improved gas savings, see https://twitter.com/transmissions11/status/1451131036377571328
///  @param a Ray
///  @param b Ray
///  @return c = a raydiv b
function rayDiv(uint256 a, uint256 b) internal pure returns (uint256 c) {
    assembly {
        if or(iszero(b), iszero(iszero(gt(a, div(sub(not(0), div(b, 2)), RAY))))) {
            revert(0, 0)
        }
        c := div(add(mul(a, RAY), div(b, 2)), b)
    }
}
```

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

### rayMul(uint256,uint256)

- **Kind**: internal
- **Source**: 2253:319:29
- **Link**: `lib/aave-v3-origin/src/contracts/protocol/libraries/math/WadRayMath.sol:WadRayMath:rayMul(uint256,uint256)`

```solidity
///  @notice Multiplies two ray, rounding half up to the nearest ray
///  @dev assembly optimized for improved gas savings, see https://twitter.com/transmissions11/status/1451131036377571328
///  @param a Ray
///  @param b Ray
///  @return c = a raymul b
function rayMul(uint256 a, uint256 b) internal pure returns (uint256 c) {
    assembly {
        if iszero(or(iszero(b), iszero(gt(a, div(sub(not(0), HALF_RAY), b))))) {
            revert(0, 0)
        }
        c := div(add(mul(a, b), HALF_RAY), RAY)
    }
}
```

### toUint128(uint256)

- **Kind**: internal
- **Source**: 1564:182:6
- **Link**: `lib/aave-v3-origin/src/contracts/dependencies/openzeppelin/contracts/SafeCast.sol:SafeCast:toUint128(uint256)`

```solidity
///  @dev Returns the downcasted uint128 from uint256, reverting on
///  overflow (when the input is greater than largest uint128).
///  Counterpart to Solidity's `uint128` operator.
///  Requirements:
///  - input must fit into 128 bits
function toUint128(uint256 value) internal pure returns (uint128) {
    require(value <= type(uint128).max, "SafeCast: value doesn't fit in 128 bits");
    return uint128(value);
}
```

### _burn(address,uint128)

- **Kind**: internal
- **Source**: 1776:520:36
- **Link**: `lib/aave-v3-origin/src/contracts/protocol/tokenization/base/MintableIncentivizedERC20.sol:MintableIncentivizedERC20:_burn(address,uint128)`

```solidity
///  @notice Burns tokens from an account and apply incentives if defined
///  @param account The account whose tokens are burnt
///  @param amount The amount of tokens to burn
function _burn(address account, uint128 amount) virtual internal {
    uint256 oldTotalSupply = _totalSupply;
    _totalSupply = oldTotalSupply - amount;
    uint128 oldAccountBalance = _userState[account].balance;
    _userState[account].balance = oldAccountBalance - amount;
    IAaveIncentivesController incentivesControllerLocal = _incentivesController;
    if (address(incentivesControllerLocal) != address(0)) {
        incentivesControllerLocal.handleAction(account, oldTotalSupply, oldAccountBalance);
    }
}
```

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
- **_userState** (`mapping(address => struct IncentivizedERC20.UserState)`)
- **POOL** (`contract IPool`) [lib/aave-v3-origin/src/contracts/interfaces/IPool.sol/interface_IPool.md]

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: AToken.burn(address,address,uint256,uint256) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: external
  ├─ [1] ⚙️ FUNCTION: ScaledBalanceTokenBase._burnScaled(address,address,uint256,uint256) (NodeID: 1)
  │   💬 Args: [from, receiverOfUnderlying, amount, index]
  │   👁️  Def: internal
  │ ├─ [2] ⚙️ FUNCTION: WadRayMath.rayDiv(uint256,uint256) (NodeID: 2)
  │ │   💬 Args: [amount, index]
  │ │   👁️  Def: internal
  │ ├─ [2] ⚙️ FUNCTION: IncentivizedERC20.balanceOf(address) (NodeID: 3)
  │ │   💬 Args: [user]
  │ │   👁️  Def: public
  │ ├─ [2] ⚙️ FUNCTION: WadRayMath.rayMul(uint256,uint256) (NodeID: 4)
  │ │   💬 Args: [scaledBalance, _userState[user].additionalData]
  │ │   👁️  Def: internal
  │ ├─ [2] ⚙️ FUNCTION: WadRayMath.rayMul(uint256,uint256) (NodeID: 5)
  │ │   💬 Args: [scaledBalance, index]
  │ │   👁️  Def: internal
  │ ├─ [2] ⚙️ FUNCTION: SafeCast.toUint128(uint256) (NodeID: 6)
  │ │   💬 Args: [index]
  │ │   👁️  Def: internal
  │ └─ [2] ⚙️ FUNCTION: MintableIncentivizedERC20._burn(address,uint128) (NodeID: 7)
  │     💬 Args: [user, amountScaled.toUint128()]
  │     👁️  Def: internal
  │   └─ [3] ⚙️ FUNCTION: SafeCast.toUint128(uint256) (NodeID: 8)
  │       💬 Args: [amountScaled]
  │       👁️  Def: internal
  └─ [1] 🔒 MODIFIER: IncentivizedERC20.onlyPool() (NodeID: 9)
      💬 Args: [no args]
    └─ [2] ⚙️ FUNCTION: Context._msgSender() (NodeID: 10)
        💬 Args: [no args]
        👁️  Def: internal
```

## Documentation

### Function Documentation

@inheritdoc IAToken

### Interface Documentation

 @notice Burns aTokens from `user` and sends the equivalent amount of underlying to `receiverOfUnderlying`
 @dev In some instances, the mint event could be emitted from a burn transaction
 if the amount to burn is less than the interest that the user accrued
 @param from The address from which the aTokens will be burned
 @param receiverOfUnderlying The address that will receive the underlying
 @param amount The amount being burned
 @param index The next liquidity index of the reserve
