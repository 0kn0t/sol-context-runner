# Function: mint(address,address,uint256,uint256)

**Contract**: [lib/aave-v3-origin/src/contracts/instances/VariableDebtTokenInstance.sol/contract_VariableDebtTokenInstance.md]

## Metadata

- **Contract**: VariableDebtTokenInstance
- **Signature**: `mint(address,address,uint256,uint256)`
- **Visibility**: external
- **Source Range**: 2277:337:32
- **Inherited From**: VariableDebtToken

## Implementation

```solidity
/// @inheritdoc IVariableDebtToken
function mint(address user, address onBehalfOf, uint256 amount, uint256 index) virtual override external onlyPool() returns (bool, uint256) {
    if (user != onBehalfOf) {
        _decreaseBorrowAllowance(onBehalfOf, user, amount);
    }
    return (_mintScaled(user, onBehalfOf, amount, index), scaledTotalSupply());
}
```

## Related Implementations

### _decreaseBorrowAllowance(address,address,uint256)

- **Kind**: internal
- **Source**: 3262:330:33
- **Link**: `lib/aave-v3-origin/src/contracts/protocol/tokenization/base/DebtTokenBase.sol:DebtTokenBase:_decreaseBorrowAllowance(address,address,uint256)`

```solidity
///  @notice Decreases the borrow allowance of a user on the specific debt token.
///  @param delegator The address delegating the borrowing power
///  @param delegatee The address receiving the delegated borrowing power
///  @param amount The amount to subtract from the current allowance
function _decreaseBorrowAllowance(address delegator, address delegatee, uint256 amount) internal {
    uint256 newAllowance = _borrowAllowances[delegator][delegatee] - amount;
    _borrowAllowances[delegator][delegatee] = newAllowance;
    emit BorrowAllowanceDelegated(delegator, delegatee, _underlyingAsset, newAllowance);
}
```

### _mintScaled(address,address,uint256,uint256)

- **Kind**: internal
- **Source**: 2290:763:37
- **Link**: `lib/aave-v3-origin/src/contracts/protocol/tokenization/base/ScaledBalanceTokenBase.sol:ScaledBalanceTokenBase:_mintScaled(address,address,uint256,uint256)`

```solidity
///  @notice Implements the basic logic to mint a scaled balance token.
///  @param caller The address performing the mint
///  @param onBehalfOf The address of the user that will receive the scaled tokens
///  @param amount The amount of tokens getting minted
///  @param index The next liquidity index of the reserve
///  @return `true` if the the previous balance of the user was 0
function _mintScaled(address caller, address onBehalfOf, uint256 amount, uint256 index) internal returns (bool) {
    uint256 amountScaled = amount.rayDiv(index);
    require(amountScaled != 0, Errors.INVALID_MINT_AMOUNT);
    uint256 scaledBalance = super.balanceOf(onBehalfOf);
    uint256 balanceIncrease = scaledBalance.rayMul(index) - scaledBalance.rayMul(_userState[onBehalfOf].additionalData);
    _userState[onBehalfOf].additionalData = index.toUint128();
    _mint(onBehalfOf, amountScaled.toUint128());
    uint256 amountToMint = amount + balanceIncrease;
    emit Transfer(address(0), onBehalfOf, amountToMint);
    emit Mint(caller, onBehalfOf, amountToMint, balanceIncrease, index);
    return (scaledBalance == 0);
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

### _mint(address,uint128)

- **Kind**: internal
- **Source**: 1064:519:36
- **Link**: `lib/aave-v3-origin/src/contracts/protocol/tokenization/base/MintableIncentivizedERC20.sol:MintableIncentivizedERC20:_mint(address,uint128)`

```solidity
///  @notice Mints tokens to an account and apply incentives if defined
///  @param account The address receiving tokens
///  @param amount The amount of tokens to mint
function _mint(address account, uint128 amount) virtual internal {
    uint256 oldTotalSupply = _totalSupply;
    _totalSupply = oldTotalSupply + amount;
    uint128 oldAccountBalance = _userState[account].balance;
    _userState[account].balance = oldAccountBalance + amount;
    IAaveIncentivesController incentivesControllerLocal = _incentivesController;
    if (address(incentivesControllerLocal) != address(0)) {
        incentivesControllerLocal.handleAction(account, oldTotalSupply, oldAccountBalance);
    }
}
```

### scaledTotalSupply()

- **Kind**: internal
- **Source**: 1596:113:37
- **Link**: `lib/aave-v3-origin/src/contracts/protocol/tokenization/base/ScaledBalanceTokenBase.sol:ScaledBalanceTokenBase:scaledTotalSupply()`

```solidity
/// @inheritdoc IScaledBalanceToken
function scaledTotalSupply() virtual override public view returns (uint256) {
    return super.totalSupply();
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

- **_borrowAllowances** (`mapping(address => mapping(address => uint256))`)
- **_underlyingAsset** (`address`)
- **_userState** (`mapping(address => struct IncentivizedERC20.UserState)`)
- **_totalSupply** (`uint256`)
- **POOL** (`contract IPool`) [lib/aave-v3-origin/src/contracts/interfaces/IPool.sol/interface_IPool.md]

## State Variable Writes

- **_borrowAllowances** (`mapping(address => mapping(address => uint256))`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: VariableDebtToken.mint(address,address,uint256,uint256) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: external
  ├─ [1] ⚙️ FUNCTION: DebtTokenBase._decreaseBorrowAllowance(address,address,uint256) (NodeID: 1)
  │   💬 Args: [onBehalfOf, user, amount]
  │   👁️  Def: internal
  ├─ [1] ⚙️ FUNCTION: ScaledBalanceTokenBase._mintScaled(address,address,uint256,uint256) (NodeID: 2)
  │   💬 Args: [user, onBehalfOf, amount, index]
  │   👁️  Def: internal
  │ ├─ [2] ⚙️ FUNCTION: WadRayMath.rayDiv(uint256,uint256) (NodeID: 3)
  │ │   💬 Args: [amount, index]
  │ │   👁️  Def: internal
  │ ├─ [2] ⚙️ FUNCTION: IncentivizedERC20.balanceOf(address) (NodeID: 4)
  │ │   💬 Args: [onBehalfOf]
  │ │   👁️  Def: public
  │ ├─ [2] ⚙️ FUNCTION: WadRayMath.rayMul(uint256,uint256) (NodeID: 5)
  │ │   💬 Args: [scaledBalance, _userState[onBehalfOf].additionalData]
  │ │   👁️  Def: internal
  │ ├─ [2] ⚙️ FUNCTION: WadRayMath.rayMul(uint256,uint256) (NodeID: 6)
  │ │   💬 Args: [scaledBalance, index]
  │ │   👁️  Def: internal
  │ ├─ [2] ⚙️ FUNCTION: SafeCast.toUint128(uint256) (NodeID: 7)
  │ │   💬 Args: [index]
  │ │   👁️  Def: internal
  │ └─ [2] ⚙️ FUNCTION: MintableIncentivizedERC20._mint(address,uint128) (NodeID: 8)
  │     💬 Args: [onBehalfOf, amountScaled.toUint128()]
  │     👁️  Def: internal
  │   └─ [3] ⚙️ FUNCTION: SafeCast.toUint128(uint256) (NodeID: 9)
  │       💬 Args: [amountScaled]
  │       👁️  Def: internal
  ├─ [1] ⚙️ FUNCTION: ScaledBalanceTokenBase.scaledTotalSupply() (NodeID: 10)
  │   💬 Args: [no args]
  │   👁️  Def: public
  │ └─ [2] ⚙️ FUNCTION: IncentivizedERC20.totalSupply() (NodeID: 11)
  │     💬 Args: [no args]
  │     👁️  Def: public
  └─ [1] 🔒 MODIFIER: IncentivizedERC20.onlyPool() (NodeID: 12)
      💬 Args: [no args]
    └─ [2] ⚙️ FUNCTION: Context._msgSender() (NodeID: 13)
        💬 Args: [no args]
        👁️  Def: internal
```

## Documentation

### Function Documentation

@inheritdoc IVariableDebtToken

### Interface Documentation

 @notice Mints debt token to the `onBehalfOf` address
 @param user The address receiving the borrowed underlying, being the delegatee in case
 of credit delegate, or same as `onBehalfOf` otherwise
 @param onBehalfOf The address receiving the debt tokens
 @param amount The amount of debt being minted
 @param index The variable debt index of the reserve
 @return True if the previous balance of the user is 0, false otherwise
 @return The scaled total debt of the reserve
