# Interface: IAToken

## Metadata

- **Name**: IAToken
- **Type**: Interface
- **Path**: lib/aave-v3-origin/src/contracts/interfaces/IAToken.sol
- **Documentation**:  @title IAToken
   @author Aave
   @notice Defines the basic interface for an AToken.

## Implements Interfaces

- **IInitializableAToken** [lib/aave-v3-origin/src/contracts/interfaces/IInitializableAToken.sol/interface_IInitializableAToken.md]
- **IScaledBalanceToken** [lib/aave-v3-origin/src/contracts/interfaces/IScaledBalanceToken.sol/interface_IScaledBalanceToken.md]
- **IERC20** [lib/aave-v3-origin/src/contracts/dependencies/openzeppelin/contracts/IERC20.sol/interface_IERC20.md]

## Events

### Transfer (inherited from IERC20)

```solidity
///  @dev Emitted when `value` tokens are moved from one account (`from`) to
///  another (`to`).
///  Note that `value` may be zero.
event Transfer(address indexed from, address indexed to, uint256 value);
```

### Approval (inherited from IERC20)

```solidity
///  @dev Emitted when the allowance of a `spender` for an `owner` is set by
///  a call to {approve}. `value` is the new allowance.
event Approval(address indexed owner, address indexed spender, uint256 value);
```

### Mint (inherited from IScaledBalanceToken)

```solidity
///  @dev Emitted after the mint action
///  @param caller The address performing the mint
///  @param onBehalfOf The address of the user that will receive the minted tokens
///  @param value The scaled-up amount being minted (based on user entered amount and balance increase from interest)
///  @param balanceIncrease The increase in scaled-up balance since the last action of 'onBehalfOf'
///  @param index The next liquidity index of the reserve
event Mint(address indexed caller, address indexed onBehalfOf, uint256 value, uint256 balanceIncrease, uint256 index);
```

### Burn (inherited from IScaledBalanceToken)

```solidity
///  @dev Emitted after the burn action
///  @dev If the burn function does not involve a transfer of the underlying asset, the target defaults to zero address
///  @param from The address from which the tokens will be burned
///  @param target The address that will receive the underlying, if any
///  @param value The scaled-up amount being burned (user entered amount - balance increase from interest)
///  @param balanceIncrease The increase in scaled-up balance since the last action of 'from'
///  @param index The next liquidity index of the reserve
event Burn(address indexed from, address indexed target, uint256 value, uint256 balanceIncrease, uint256 index);
```

### Initialized (inherited from IInitializableAToken)

```solidity
///  @dev Emitted when an aToken is initialized
///  @param underlyingAsset The address of the underlying asset
///  @param pool The address of the associated pool
///  @param treasury The address of the treasury
///  @param incentivesController The address of the incentives controller for this aToken
///  @param aTokenDecimals The decimals of the underlying
///  @param aTokenName The name of the aToken
///  @param aTokenSymbol The symbol of the aToken
///  @param params A set of encoded parameters for additional initialization
event Initialized(address indexed underlyingAsset, address indexed pool, address treasury, address incentivesController, uint8 aTokenDecimals, string aTokenName, string aTokenSymbol, bytes params);
```

### BalanceTransfer

```solidity
///  @dev Emitted during the transfer action
///  @param from The user whose tokens are being transferred
///  @param to The recipient
///  @param value The scaled amount being transferred
///  @param index The next liquidity index of the reserve
event BalanceTransfer(address indexed from, address indexed to, uint256 value, uint256 index);
```

## Public/External Functions

### mint(address,address,uint256,uint256)

- **Signature**: `mint(address,address,uint256,uint256)`
- **Visibility**: external
- **Source Range**: 1160:125:13

**Signature:**
```solidity
///  @notice Mints `amount` aTokens to `user`
///  @param caller The address performing the mint
///  @param onBehalfOf The address of the user that will receive the minted aTokens
///  @param amount The amount of tokens getting minted
///  @param index The next liquidity index of the reserve
///  @return `true` if the the previous balance of the user was 0
function mint(address caller, address onBehalfOf, uint256 amount, uint256 index) external returns (bool);;
```

### burn(address,address,uint256,uint256)

- **Signature**: `burn(address,address,uint256,uint256)`
- **Visibility**: external
- **Source Range**: 1818:98:13

**Signature:**
```solidity
///  @notice Burns aTokens from `user` and sends the equivalent amount of underlying to `receiverOfUnderlying`
///  @dev In some instances, the mint event could be emitted from a burn transaction
///  if the amount to burn is less than the interest that the user accrued
///  @param from The address from which the aTokens will be burned
///  @param receiverOfUnderlying The address that will receive the underlying
///  @param amount The amount being burned
///  @param index The next liquidity index of the reserve
function burn(address from, address receiverOfUnderlying, uint256 amount, uint256 index) external;;
```

### mintToTreasury(uint256,uint256)

- **Signature**: `mintToTreasury(uint256,uint256)`
- **Visibility**: external
- **Source Range**: 2096:64:13

**Signature:**
```solidity
///  @notice Mints aTokens to the reserve treasury
///  @param amount The amount of tokens getting minted
///  @param index The next liquidity index of the reserve
function mintToTreasury(uint256 amount, uint256 index) external;;
```

### transferOnLiquidation(address,address,uint256)

- **Signature**: `transferOnLiquidation(address,address,uint256)`
- **Visibility**: external
- **Source Range**: 2460:81:13

**Signature:**
```solidity
///  @notice Transfers aTokens in the event of a borrow being liquidated, in case the liquidators reclaims the aToken
///  @param from The address getting liquidated, current owner of the aTokens
///  @param to The recipient
///  @param value The amount of tokens getting transferred
function transferOnLiquidation(address from, address to, uint256 value) external;;
```

### transferUnderlyingTo(address,uint256)

- **Signature**: `transferUnderlyingTo(address,uint256)`
- **Visibility**: external
- **Source Range**: 2801:71:13

**Signature:**
```solidity
///  @notice Transfers the underlying asset to `target`.
///  @dev Used by the Pool to transfer assets in borrow(), withdraw() and flashLoan()
///  @param target The recipient of the underlying
///  @param amount The amount getting transferred
function transferUnderlyingTo(address target, uint256 amount) external;;
```

### handleRepayment(address,address,uint256)

- **Signature**: `handleRepayment(address,address,uint256)`
- **Visibility**: external
- **Source Range**: 3509:84:13

**Signature:**
```solidity
///  @notice Handles the underlying received by the aToken after the transfer has been completed.
///  @dev The default implementation is empty as with standard ERC20 tokens, nothing needs to be done after the
///  transfer is concluded. However in the future there may be aTokens that allow for example to stake the underlying
///  to receive LM rewards. In that case, `handleRepayment()` would perform the staking of the underlying asset.
///  @param user The user executing the repayment
///  @param onBehalfOf The address of the user who will get his debt reduced/removed
///  @param amount The amount getting repaid
function handleRepayment(address user, address onBehalfOf, uint256 amount) external;;
```

### permit(address,address,uint256,uint256,uint8,bytes32,bytes32)

- **Signature**: `permit(address,address,uint256,uint256,uint8,bytes32,bytes32)`
- **Visibility**: external
- **Source Range**: 4094:153:13

**Signature:**
```solidity
///  @notice Allow passing a signed message to approve spending
///  @dev implements the permit function as for
///  https://github.com/ethereum/EIPs/blob/8a34d644aacf0f9f8f00815307fd7dd5da07655f/EIPS/eip-2612.md
///  @param owner The owner of the funds
///  @param spender The spender
///  @param value The amount
///  @param deadline The deadline timestamp, type(uint256).max for max deadline
///  @param v Signature param
///  @param s Signature param
///  @param r Signature param
function permit(address owner, address spender, uint256 value, uint256 deadline, uint8 v, bytes32 r, bytes32 s) external;;
```

### UNDERLYING_ASSET_ADDRESS()

- **Signature**: `UNDERLYING_ASSET_ADDRESS()`
- **Visibility**: external
- **Source Range**: 4406:68:13

**Signature:**
```solidity
///  @notice Returns the address of the underlying asset of this aToken (E.g. WETH for aWETH)
///  @return The address of the underlying asset
function UNDERLYING_ASSET_ADDRESS() external view returns (address);;
```

### RESERVE_TREASURY_ADDRESS()

- **Signature**: `RESERVE_TREASURY_ADDRESS()`
- **Visibility**: external
- **Source Range**: 4622:68:13

**Signature:**
```solidity
///  @notice Returns the address of the Aave treasury, receiving the fees on this aToken.
///  @return Address of the Aave treasury
function RESERVE_TREASURY_ADDRESS() external view returns (address);;
```

### DOMAIN_SEPARATOR()

- **Signature**: `DOMAIN_SEPARATOR()`
- **Visibility**: external
- **Source Range**: 4909:60:13

**Signature:**
```solidity
///  @notice Get the domain separator for the token
///  @dev Return cached value if chainId matches cache, otherwise recomputes separator
///  @return The domain separator of the token at current chain
function DOMAIN_SEPARATOR() external view returns (bytes32);;
```

### nonces(address)

- **Signature**: `nonces(address)`
- **Visibility**: external
- **Source Range**: 5106:63:13

**Signature:**
```solidity
///  @notice Returns the nonce for owner.
///  @param owner The address of the owner
///  @return The nonce of the owner
function nonces(address owner) external view returns (uint256);;
```

### rescueTokens(address,address,uint256)

- **Signature**: `rescueTokens(address,address,uint256)`
- **Visibility**: external
- **Source Range**: 5387:74:13

**Signature:**
```solidity
///  @notice Rescue and transfer tokens locked in this contract
///  @param token The address of the token
///  @param to The address of the recipient
///  @param amount The amount of token to transfer
function rescueTokens(address token, address to, uint256 amount) external;;
```

### totalSupply() (inherited from IERC20)

- **Signature**: `totalSupply()`
- **Visibility**: external
- **Source Range**: 214:55:3

**Signature:**
```solidity
///  @dev Returns the amount of tokens in existence.
function totalSupply() external view returns (uint256);;
```

### balanceOf(address) (inherited from IERC20)

- **Signature**: `balanceOf(address)`
- **Visibility**: external
- **Source Range**: 344:68:3

**Signature:**
```solidity
///  @dev Returns the amount of tokens owned by `account`.
function balanceOf(address account) external view returns (uint256);;
```

### transfer(address,uint256) (inherited from IERC20)

- **Signature**: `transfer(address,uint256)`
- **Visibility**: external
- **Source Range**: 616:77:3

**Signature:**
```solidity
///  @dev Moves `amount` tokens from the caller's account to `recipient`.
///  Returns a boolean value indicating whether the operation succeeded.
///  Emits a {Transfer} event.
function transfer(address recipient, uint256 amount) external returns (bool);;
```

### allowance(address,address) (inherited from IERC20)

- **Signature**: `allowance(address,address)`
- **Visibility**: external
- **Source Range**: 952:83:3

**Signature:**
```solidity
///  @dev Returns the remaining number of tokens that `spender` will be
///  allowed to spend on behalf of `owner` through {transferFrom}. This is
///  zero by default.
///  This value changes when {approve} or {transferFrom} are called.
function allowance(address owner, address spender) external view returns (uint256);;
```

### approve(address,uint256) (inherited from IERC20)

- **Signature**: `approve(address,uint256)`
- **Visibility**: external
- **Source Range**: 1658:74:3

**Signature:**
```solidity
///  @dev Sets `amount` as the allowance of `spender` over the caller's tokens.
///  Returns a boolean value indicating whether the operation succeeded.
///  IMPORTANT: Beware that changing an allowance with this method brings the risk
///  that someone may use both the old and the new allowance by unfortunate
///  transaction ordering. One possible solution to mitigate this race
///  condition is to first reduce the spender's allowance to 0 and set the
///  desired value afterwards:
///  https://github.com/ethereum/EIPs/issues/20#issuecomment-263524729
///  Emits an {Approval} event.
function approve(address spender, uint256 amount) external returns (bool);;
```

### transferFrom(address,address,uint256) (inherited from IERC20)

- **Signature**: `transferFrom(address,address,uint256)`
- **Visibility**: external
- **Source Range**: 2019:97:3

**Signature:**
```solidity
///  @dev Moves `amount` tokens from `sender` to `recipient` using the
///  allowance mechanism. `amount` is then deducted from the caller's
///  allowance.
///  Returns a boolean value indicating whether the operation succeeded.
///  Emits a {Transfer} event.
function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);;
```

### scaledBalanceOf(address) (inherited from IScaledBalanceToken)

- **Signature**: `scaledBalanceOf(address)`
- **Visibility**: external
- **Source Range**: 1840:71:20

**Signature:**
```solidity
///  @notice Returns the scaled balance of the user.
///  @dev The scaled balance is the sum of all the updated stored balance divided by the reserve's liquidity index
///  at the moment of the update
///  @param user The user whose balance is calculated
///  @return The scaled balance of the user
function scaledBalanceOf(address user) external view returns (uint256);;
```

### getScaledUserBalanceAndSupply(address) (inherited from IScaledBalanceToken)

- **Signature**: `getScaledUserBalanceAndSupply(address)`
- **Visibility**: external
- **Source Range**: 2130:94:20

**Signature:**
```solidity
///  @notice Returns the scaled balance of the user and the scaled total supply.
///  @param user The address of the user
///  @return The scaled balance of the user
///  @return The scaled total supply
function getScaledUserBalanceAndSupply(address user) external view returns (uint256, uint256);;
```

### scaledTotalSupply() (inherited from IScaledBalanceToken)

- **Signature**: `scaledTotalSupply()`
- **Visibility**: external
- **Source Range**: 2378:61:20

**Signature:**
```solidity
///  @notice Returns the scaled total supply of the scaled balance token. Represents sum(debt/index)
///  @return The scaled total supply
function scaledTotalSupply() external view returns (uint256);;
```

### getPreviousIndex(address) (inherited from IScaledBalanceToken)

- **Signature**: `getPreviousIndex(address)`
- **Visibility**: external
- **Source Range**: 2660:72:20

**Signature:**
```solidity
///  @notice Returns last index interest was accrued to the user's balance
///  @param user The address of the user
///  @return The last index interest was accrued to the user's balance, expressed in ray
function getPreviousIndex(address user) external view returns (uint256);;
```

### initialize(contract IPool,address,address,contract IAaveIncentivesController,uint8,string,string,bytes) (inherited from IInitializableAToken)

- **Signature**: `initialize(contract IPool,address,address,contract IAaveIncentivesController,uint8,string,string,bytes)`
- **Visibility**: external
- **Source Range**: 1762:271:16

**Signature:**
```solidity
///  @notice Initializes the aToken
///  @param pool The pool contract that is initializing this contract
///  @param treasury The address of the Aave treasury, receiving the fees on this aToken
///  @param underlyingAsset The address of the underlying asset of this aToken (E.g. WETH for aWETH)
///  @param incentivesController The smart contract managing potential incentives distribution
///  @param aTokenDecimals The decimals of the aToken, same as the underlying asset's
///  @param aTokenName The name of the aToken
///  @param aTokenSymbol The symbol of the aToken
///  @param params A set of encoded parameters for additional initialization
function initialize(IPool pool, address treasury, address underlyingAsset, IAaveIncentivesController incentivesController, uint8 aTokenDecimals, string calldata aTokenName, string calldata aTokenSymbol, bytes calldata params) external;;
```
