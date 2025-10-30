# Interface: IVariableDebtToken

## Metadata

- **Name**: IVariableDebtToken
- **Type**: Interface
- **Path**: lib/aave-v3-origin/src/contracts/interfaces/IVariableDebtToken.sol
- **Documentation**:  @title IVariableDebtToken
   @author Aave
   @notice Defines the basic interface for a variable debt token.

## Implements Interfaces

- **IInitializableDebtToken** [lib/aave-v3-origin/src/contracts/interfaces/IInitializableDebtToken.sol/interface_IInitializableDebtToken.md]
- **IScaledBalanceToken** [lib/aave-v3-origin/src/contracts/interfaces/IScaledBalanceToken.sol/interface_IScaledBalanceToken.md]

## Events

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

### Initialized (inherited from IInitializableDebtToken)

```solidity
///  @dev Emitted when a debt token is initialized
///  @param underlyingAsset The address of the underlying asset
///  @param pool The address of the associated pool
///  @param incentivesController The address of the incentives controller for this aToken
///  @param debtTokenDecimals The decimals of the debt token
///  @param debtTokenName The name of the debt token
///  @param debtTokenSymbol The symbol of the debt token
///  @param params A set of encoded parameters for additional initialization
event Initialized(address indexed underlyingAsset, address indexed pool, address incentivesController, uint8 debtTokenDecimals, string debtTokenName, string debtTokenSymbol, bytes params);
```

## Public/External Functions

### mint(address,address,uint256,uint256)

- **Signature**: `mint(address,address,uint256,uint256)`
- **Visibility**: external
- **Source Range**: 908:132:21

**Signature:**
```solidity
///  @notice Mints debt token to the `onBehalfOf` address
///  @param user The address receiving the borrowed underlying, being the delegatee in case
///  of credit delegate, or same as `onBehalfOf` otherwise
///  @param onBehalfOf The address receiving the debt tokens
///  @param amount The amount of debt being minted
///  @param index The variable debt index of the reserve
///  @return True if the previous balance of the user is 0, false otherwise
///  @return The scaled total debt of the reserve
function mint(address user, address onBehalfOf, uint256 amount, uint256 index) external returns (bool, uint256);;
```

### burn(address,uint256,uint256)

- **Signature**: `burn(address,uint256,uint256)`
- **Visibility**: external
- **Source Range**: 1456:86:21

**Signature:**
```solidity
///  @notice Burns user variable debt
///  @dev In some instances, a burn transaction will emit a mint event
///  if the amount to burn is less than the interest that the user accrued
///  @param from The address from which the debt will be burned
///  @param amount The amount getting burned
///  @param index The variable debt index of the reserve
///  @return The scaled total debt of the reserve
function burn(address from, uint256 amount, uint256 index) external returns (uint256);;
```

### UNDERLYING_ASSET_ADDRESS()

- **Signature**: `UNDERLYING_ASSET_ADDRESS()`
- **Visibility**: external
- **Source Range**: 1715:68:21

**Signature:**
```solidity
///  @notice Returns the address of the underlying asset of this debtToken (E.g. WETH for variableDebtWETH)
///  @return The address of the underlying asset
function UNDERLYING_ASSET_ADDRESS() external view returns (address);;
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

### initialize(contract IPool,address,contract IAaveIncentivesController,uint8,string,string,bytes) (inherited from IInitializableDebtToken)

- **Signature**: `initialize(contract IPool,address,contract IAaveIncentivesController,uint8,string,string,bytes)`
- **Visibility**: external
- **Source Range**: 1669:254:17

**Signature:**
```solidity
///  @notice Initializes the debt token.
///  @param pool The pool contract that is initializing this contract
///  @param underlyingAsset The address of the underlying asset of this aToken (E.g. WETH for aWETH)
///  @param incentivesController The smart contract managing potential incentives distribution
///  @param debtTokenDecimals The decimals of the debtToken, same as the underlying asset's
///  @param debtTokenName The name of the token
///  @param debtTokenSymbol The symbol of the token
///  @param params A set of encoded parameters for additional initialization
function initialize(IPool pool, address underlyingAsset, IAaveIncentivesController incentivesController, uint8 debtTokenDecimals, string memory debtTokenName, string memory debtTokenSymbol, bytes calldata params) external;;
```
