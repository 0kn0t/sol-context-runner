# Function: transferOnLiquidation(address,address,uint256)

**Contract**: [lib/aave-v3-origin/src/contracts/instances/ATokenInstance.sol/contract_ATokenInstance.md]

## Metadata

- **Contract**: ATokenInstance
- **Signature**: `transferOnLiquidation(address,address,uint256)`
- **Visibility**: external
- **Source Range**: 2956:296:31
- **Inherited From**: AToken

## Implementation

```solidity
/// @inheritdoc IAToken
function transferOnLiquidation(address from, address to, uint256 value) virtual override external onlyPool() {
    _transfer(from, to, value, false);
}
```

## Related Implementations

### _transfer(address,address,uint256,bool)

- **Kind**: internal
- **Source**: 5633:592:31
- **Link**: `lib/aave-v3-origin/src/contracts/protocol/tokenization/AToken.sol:AToken:_transfer(address,address,uint256,bool)`

```solidity
///  @notice Transfers the aTokens between two users. Validates the transfer
///  (ie checks for valid HF after the transfer) if required
///  @param from The source address
///  @param to The destination address
///  @param amount The amount getting transferred
///  @param validate True if the transfer needs to be validated, false otherwise
function _transfer(address from, address to, uint256 amount, bool validate) virtual internal {
    address underlyingAsset = _underlyingAsset;
    uint256 index = POOL.getReserveNormalizedIncome(underlyingAsset);
    uint256 fromBalanceBefore = super.balanceOf(from).rayMul(index);
    uint256 toBalanceBefore = super.balanceOf(to).rayMul(index);
    super._transfer(from, to, amount, index);
    if (validate) {
        POOL.finalizeTransfer(underlyingAsset, from, to, amount, fromBalanceBefore, toBalanceBefore);
    }
    emit BalanceTransfer(from, to, amount.rayDiv(index), index);
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

### _transfer(address,address,uint256,uint256)

- **Kind**: internal
- **Source**: 4762:1203:37
- **Link**: `lib/aave-v3-origin/src/contracts/protocol/tokenization/base/ScaledBalanceTokenBase.sol:ScaledBalanceTokenBase:_transfer(address,address,uint256,uint256)`

```solidity
///  @notice Implements the basic logic to transfer scaled balance tokens between two users
///  @dev It emits a mint event with the interest accrued per user
///  @param sender The source address
///  @param recipient The destination address
///  @param amount The amount getting transferred
///  @param index The next liquidity index of the reserve
function _transfer(address sender, address recipient, uint256 amount, uint256 index) internal {
    uint256 senderScaledBalance = super.balanceOf(sender);
    uint256 senderBalanceIncrease = senderScaledBalance.rayMul(index) - senderScaledBalance.rayMul(_userState[sender].additionalData);
    uint256 recipientScaledBalance = super.balanceOf(recipient);
    uint256 recipientBalanceIncrease = recipientScaledBalance.rayMul(index) - recipientScaledBalance.rayMul(_userState[recipient].additionalData);
    _userState[sender].additionalData = index.toUint128();
    _userState[recipient].additionalData = index.toUint128();
    super._transfer(sender, recipient, amount.rayDiv(index).toUint128());
    if (senderBalanceIncrease > 0) {
        emit Transfer(address(0), sender, senderBalanceIncrease);
        emit Mint(_msgSender(), sender, senderBalanceIncrease, senderBalanceIncrease, index);
    }
    if ((sender != recipient) && (recipientBalanceIncrease > 0)) {
        emit Transfer(address(0), recipient, recipientBalanceIncrease);
        emit Mint(_msgSender(), recipient, recipientBalanceIncrease, recipientBalanceIncrease, index);
    }
    emit Transfer(sender, recipient, amount);
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

### _transfer(address,address,uint128)

- **Kind**: internal
- **Source**: 6149:772:35
- **Link**: `lib/aave-v3-origin/src/contracts/protocol/tokenization/base/IncentivizedERC20.sol:IncentivizedERC20:_transfer(address,address,uint128)`

```solidity
///  @notice Transfers tokens between two users and apply incentives if defined.
///  @param sender The source address
///  @param recipient The destination address
///  @param amount The amount getting transferred
function _transfer(address sender, address recipient, uint128 amount) virtual internal {
    uint128 oldSenderBalance = _userState[sender].balance;
    _userState[sender].balance = oldSenderBalance - amount;
    uint128 oldRecipientBalance = _userState[recipient].balance;
    _userState[recipient].balance = oldRecipientBalance + amount;
    IAaveIncentivesController incentivesControllerLocal = _incentivesController;
    if (address(incentivesControllerLocal) != address(0)) {
        uint256 currentTotalSupply = _totalSupply;
        incentivesControllerLocal.handleAction(sender, currentTotalSupply, oldSenderBalance);
        if (sender != recipient) {
            incentivesControllerLocal.handleAction(recipient, currentTotalSupply, oldRecipientBalance);
        }
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

### _msgSender()

- **Kind**: internal
- **Source**: 588:107:2
- **Link**: `lib/aave-v3-origin/src/contracts/dependencies/openzeppelin/contracts/Context.sol:Context:_msgSender()`

```solidity
function _msgSender() virtual internal view returns (address payable) {
    return payable(msg.sender);
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

## State Variable Reads

- **_underlyingAsset** (`address`)
- **_userState** (`mapping(address => struct IncentivizedERC20.UserState)`)
- **_incentivesController** (`contract IAaveIncentivesController`) [lib/aave-v3-origin/src/contracts/interfaces/IAaveIncentivesController.sol/interface_IAaveIncentivesController.md]
- **_totalSupply** (`uint256`)
- **POOL** (`contract IPool`) [lib/aave-v3-origin/src/contracts/interfaces/IPool.sol/interface_IPool.md]

## State Variable Writes

- **_userState** (`mapping(address => struct IncentivizedERC20.UserState)`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: AToken.transferOnLiquidation(address,address,uint256) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: external
  ├─ [1] ⚙️ FUNCTION: AToken._transfer(address,address,uint256,bool) (NodeID: 1)
  │   💬 Args: [from, to, value, false]
  │   👁️  Def: internal
  │ ├─ [2] ⚙️ FUNCTION: IncentivizedERC20.balanceOf(address) (NodeID: 2)
  │ │   💬 Args: [from]
  │ │   👁️  Def: public
  │ ├─ [2] ⚙️ FUNCTION: WadRayMath.rayMul(uint256,uint256) (NodeID: 3)
  │ │   💬 Args: [super.balanceOf(from), index]
  │ │   👁️  Def: internal
  │ ├─ [2] ⚙️ FUNCTION: IncentivizedERC20.balanceOf(address) (NodeID: 4)
  │ │   💬 Args: [to]
  │ │   👁️  Def: public
  │ ├─ [2] ⚙️ FUNCTION: WadRayMath.rayMul(uint256,uint256) (NodeID: 5)
  │ │   💬 Args: [super.balanceOf(to), index]
  │ │   👁️  Def: internal
  │ ├─ [2] ⚙️ FUNCTION: ScaledBalanceTokenBase._transfer(address,address,uint256,uint256) (NodeID: 6)
  │ │   💬 Args: [from, to, amount, index]
  │ │   👁️  Def: internal
  │ │ ├─ [3] ⚙️ FUNCTION: IncentivizedERC20.balanceOf(address) (NodeID: 7)
  │ │ │   💬 Args: [sender]
  │ │ │   👁️  Def: public
  │ │ ├─ [3] ⚙️ FUNCTION: WadRayMath.rayMul(uint256,uint256) (NodeID: 8)
  │ │ │   💬 Args: [senderScaledBalance, _userState[sender].additionalData]
  │ │ │   👁️  Def: internal
  │ │ ├─ [3] ⚙️ FUNCTION: WadRayMath.rayMul(uint256,uint256) (NodeID: 9)
  │ │ │   💬 Args: [senderScaledBalance, index]
  │ │ │   👁️  Def: internal
  │ │ ├─ [3] ⚙️ FUNCTION: IncentivizedERC20.balanceOf(address) (NodeID: 10)
  │ │ │   💬 Args: [recipient]
  │ │ │   👁️  Def: public
  │ │ ├─ [3] ⚙️ FUNCTION: WadRayMath.rayMul(uint256,uint256) (NodeID: 11)
  │ │ │   💬 Args: [recipientScaledBalance, _userState[recipient].additionalData]
  │ │ │   👁️  Def: internal
  │ │ ├─ [3] ⚙️ FUNCTION: WadRayMath.rayMul(uint256,uint256) (NodeID: 12)
  │ │ │   💬 Args: [recipientScaledBalance, index]
  │ │ │   👁️  Def: internal
  │ │ ├─ [3] ⚙️ FUNCTION: SafeCast.toUint128(uint256) (NodeID: 13)
  │ │ │   💬 Args: [index]
  │ │ │   👁️  Def: internal
  │ │ ├─ [3] ⚙️ FUNCTION: SafeCast.toUint128(uint256) (NodeID: 14)
  │ │ │   💬 Args: [index]
  │ │ │   👁️  Def: internal
  │ │ ├─ [3] ⚙️ FUNCTION: IncentivizedERC20._transfer(address,address,uint128) (NodeID: 15)
  │ │ │   💬 Args: [sender, recipient, amount.rayDiv(index).toUint128()]
  │ │ │   👁️  Def: internal
  │ │ │ ├─ [4] ⚙️ FUNCTION: WadRayMath.rayDiv(uint256,uint256) (NodeID: 16)
  │ │ │ │   💬 Args: [amount, index]
  │ │ │ │   👁️  Def: internal
  │ │ │ └─ [4] ⚙️ FUNCTION: SafeCast.toUint128(uint256) (NodeID: 17)
  │ │ │     💬 Args: [amount.rayDiv(index)]
  │ │ │     👁️  Def: internal
  │ │ ├─ [3] ⚙️ FUNCTION: Context._msgSender() (NodeID: 18)
  │ │ │   💬 Args: [no args]
  │ │ │   👁️  Def: internal
  │ │ └─ [3] ⚙️ FUNCTION: Context._msgSender() (NodeID: 19)
  │ │     💬 Args: [no args]
  │ │     👁️  Def: internal
  │ └─ [2] ⚙️ FUNCTION: WadRayMath.rayDiv(uint256,uint256) (NodeID: 20)
  │     💬 Args: [amount, index]
  │     👁️  Def: internal
  └─ [1] 🔒 MODIFIER: IncentivizedERC20.onlyPool() (NodeID: 21)
      💬 Args: [no args]
    └─ [2] ⚙️ FUNCTION: Context._msgSender() (NodeID: 22)
        💬 Args: [no args]
        👁️  Def: internal
```

## Documentation

### Function Documentation

@inheritdoc IAToken

### Interface Documentation

 @notice Transfers aTokens in the event of a borrow being liquidated, in case the liquidators reclaims the aToken
 @param from The address getting liquidated, current owner of the aTokens
 @param to The recipient
 @param value The amount of tokens getting transferred
