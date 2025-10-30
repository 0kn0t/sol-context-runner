# Interface: IPool

## Metadata

- **Name**: IPool
- **Type**: Interface
- **Path**: lib/aave-v3-origin/src/contracts/interfaces/IPool.sol
- **Documentation**:  @title IPool
   @author Aave
   @notice Defines the basic interface for an Aave Pool.

## Events

### MintUnbacked

```solidity
///  @dev Emitted on mintUnbacked()
///  @param reserve The address of the underlying asset of the reserve
///  @param user The address initiating the supply
///  @param onBehalfOf The beneficiary of the supplied assets, receiving the aTokens
///  @param amount The amount of supplied assets
///  @param referralCode The referral code used
event MintUnbacked(address indexed reserve, address user, address indexed onBehalfOf, uint256 amount, uint16 indexed referralCode);
```

### BackUnbacked

```solidity
///  @dev Emitted on backUnbacked()
///  @param reserve The address of the underlying asset of the reserve
///  @param backer The address paying for the backing
///  @param amount The amount added as backing
///  @param fee The amount paid in fees
event BackUnbacked(address indexed reserve, address indexed backer, uint256 amount, uint256 fee);
```

### Supply

```solidity
///  @dev Emitted on supply()
///  @param reserve The address of the underlying asset of the reserve
///  @param user The address initiating the supply
///  @param onBehalfOf The beneficiary of the supply, receiving the aTokens
///  @param amount The amount supplied
///  @param referralCode The referral code used
event Supply(address indexed reserve, address user, address indexed onBehalfOf, uint256 amount, uint16 indexed referralCode);
```

### Withdraw

```solidity
///  @dev Emitted on withdraw()
///  @param reserve The address of the underlying asset being withdrawn
///  @param user The address initiating the withdrawal, owner of aTokens
///  @param to The address that will receive the underlying
///  @param amount The amount to be withdrawn
event Withdraw(address indexed reserve, address indexed user, address indexed to, uint256 amount);
```

### Borrow

```solidity
///  @dev Emitted on borrow() and flashLoan() when debt needs to be opened
///  @param reserve The address of the underlying asset being borrowed
///  @param user The address of the user initiating the borrow(), receiving the funds on borrow() or just
///  initiator of the transaction on flashLoan()
///  @param onBehalfOf The address that will be getting the debt
///  @param amount The amount borrowed out
///  @param interestRateMode The rate mode: 2 for Variable, 1 is deprecated (changed on v3.2.0)
///  @param borrowRate The numeric rate at which the user has borrowed, expressed in ray
///  @param referralCode The referral code used
event Borrow(address indexed reserve, address user, address indexed onBehalfOf, uint256 amount, DataTypes.InterestRateMode interestRateMode, uint256 borrowRate, uint16 indexed referralCode);
```

### Repay

```solidity
///  @dev Emitted on repay()
///  @param reserve The address of the underlying asset of the reserve
///  @param user The beneficiary of the repayment, getting his debt reduced
///  @param repayer The address of the user initiating the repay(), providing the funds
///  @param amount The amount repaid
///  @param useATokens True if the repayment is done using aTokens, `false` if done with underlying asset directly
event Repay(address indexed reserve, address indexed user, address indexed repayer, uint256 amount, bool useATokens);
```

### IsolationModeTotalDebtUpdated

```solidity
///  @dev Emitted on borrow(), repay() and liquidationCall() when using isolated assets
///  @param asset The address of the underlying asset of the reserve
///  @param totalDebt The total isolation mode debt for the reserve
event IsolationModeTotalDebtUpdated(address indexed asset, uint256 totalDebt);
```

### UserEModeSet

```solidity
///  @dev Emitted when the user selects a certain asset category for eMode
///  @param user The address of the user
///  @param categoryId The category id
event UserEModeSet(address indexed user, uint8 categoryId);
```

### ReserveUsedAsCollateralEnabled

```solidity
///  @dev Emitted on setUserUseReserveAsCollateral()
///  @param reserve The address of the underlying asset of the reserve
///  @param user The address of the user enabling the usage as collateral
event ReserveUsedAsCollateralEnabled(address indexed reserve, address indexed user);
```

### ReserveUsedAsCollateralDisabled

```solidity
///  @dev Emitted on setUserUseReserveAsCollateral()
///  @param reserve The address of the underlying asset of the reserve
///  @param user The address of the user enabling the usage as collateral
event ReserveUsedAsCollateralDisabled(address indexed reserve, address indexed user);
```

### FlashLoan

```solidity
///  @dev Emitted on flashLoan()
///  @param target The address of the flash loan receiver contract
///  @param initiator The address initiating the flash loan
///  @param asset The address of the asset being flash borrowed
///  @param amount The amount flash borrowed
///  @param interestRateMode The flashloan mode: 0 for regular flashloan,
///         1 for Stable (Deprecated on v3.2.0), 2 for Variable
///  @param premium The fee flash borrowed
///  @param referralCode The referral code used
event FlashLoan(address indexed target, address initiator, address indexed asset, uint256 amount, DataTypes.InterestRateMode interestRateMode, uint256 premium, uint16 indexed referralCode);
```

### LiquidationCall

```solidity
///  @dev Emitted when a borrower is liquidated.
///  @param collateralAsset The address of the underlying asset used as collateral, to receive as result of the liquidation
///  @param debtAsset The address of the underlying borrowed asset to be repaid with the liquidation
///  @param user The address of the borrower getting liquidated
///  @param debtToCover The debt amount of borrowed `asset` the liquidator wants to cover
///  @param liquidatedCollateralAmount The amount of collateral received by the liquidator
///  @param liquidator The address of the liquidator
///  @param receiveAToken True if the liquidators wants to receive the collateral aTokens, `false` if he wants
///  to receive the underlying collateral asset directly
event LiquidationCall(address indexed collateralAsset, address indexed debtAsset, address indexed user, uint256 debtToCover, uint256 liquidatedCollateralAmount, address liquidator, bool receiveAToken);
```

### ReserveDataUpdated

```solidity
///  @dev Emitted when the state of a reserve is updated.
///  @param reserve The address of the underlying asset of the reserve
///  @param liquidityRate The next liquidity rate
///  @param stableBorrowRate The next stable borrow rate @note deprecated on v3.2.0
///  @param variableBorrowRate The next variable borrow rate
///  @param liquidityIndex The next liquidity index
///  @param variableBorrowIndex The next variable borrow index
event ReserveDataUpdated(address indexed reserve, uint256 liquidityRate, uint256 stableBorrowRate, uint256 variableBorrowRate, uint256 liquidityIndex, uint256 variableBorrowIndex);
```

### DeficitCovered

```solidity
///  @dev Emitted when the deficit of a reserve is covered.
///  @param reserve The address of the underlying asset of the reserve
///  @param caller The caller that triggered the DeficitCovered event
///  @param amountCovered The amount of deficit covered
event DeficitCovered(address indexed reserve, address caller, uint256 amountCovered);
```

### MintedToTreasury

```solidity
///  @dev Emitted when the protocol treasury receives minted aTokens from the accrued interest.
///  @param reserve The address of the reserve
///  @param amountMinted The amount minted to the treasury
event MintedToTreasury(address indexed reserve, uint256 amountMinted);
```

### DeficitCreated

```solidity
///  @dev Emitted when deficit is realized on a liquidation.
///  @param user The user address where the bad debt will be burned
///  @param debtAsset The address of the underlying borrowed asset to be burned
///  @param amountCreated The amount of deficit created
event DeficitCreated(address indexed user, address indexed debtAsset, uint256 amountCreated);
```

## Public/External Functions

### mintUnbacked(address,uint256,address,uint16)

- **Signature**: `mintUnbacked(address,uint256,address,uint16)`
- **Visibility**: external
- **Source Range**: 8499:123:4

**Signature:**
```solidity
///  @notice Mints an `amount` of aTokens to the `onBehalfOf`
///  @param asset The address of the underlying asset to mint
///  @param amount The amount to mint
///  @param onBehalfOf The address that will receive the aTokens
///  @param referralCode Code used to register the integrator originating the operation, for potential rewards.
///    0 if the action is executed directly by the user, without any middle-man
function mintUnbacked(address asset, uint256 amount, address onBehalfOf, uint16 referralCode) external;;
```

### backUnbacked(address,uint256,uint256)

- **Signature**: `backUnbacked(address,uint256,uint256)`
- **Visibility**: external
- **Source Range**: 8888:93:4

**Signature:**
```solidity
///  @notice Back the current unbacked underlying with `amount` and pay `fee`.
///  @param asset The address of the underlying asset to back
///  @param amount The amount to back
///  @param fee The amount paid in fees
///  @return The backed amount
function backUnbacked(address asset, uint256 amount, uint256 fee) external returns (uint256);;
```

### supply(address,uint256,address,uint16)

- **Signature**: `supply(address,uint256,address,uint16)`
- **Visibility**: external
- **Source Range**: 9700:97:4

**Signature:**
```solidity
///  @notice Supplies an `amount` of underlying asset into the reserve, receiving in return overlying aTokens.
///  - E.g. User supplies 100 USDC and gets in return 100 aUSDC
///  @param asset The address of the underlying asset to supply
///  @param amount The amount to be supplied
///  @param onBehalfOf The address that will receive the aTokens, same as msg.sender if the user
///    wants to receive them on his own wallet, or a different address if the beneficiary of aTokens
///    is a different wallet
///  @param referralCode Code used to register the integrator originating the operation, for potential rewards.
///    0 if the action is executed directly by the user, without any middle-man
function supply(address asset, uint256 amount, address onBehalfOf, uint16 referralCode) external;;
```

### supplyWithPermit(address,uint256,address,uint16,uint256,uint8,bytes32,bytes32)

- **Signature**: `supplyWithPermit(address,uint256,address,uint16,uint256,uint8,bytes32,bytes32)`
- **Visibility**: external
- **Source Range**: 10766:210:4

**Signature:**
```solidity
///  @notice Supply with transfer approval of asset to be supplied done via permit function
///  see: https://eips.ethereum.org/EIPS/eip-2612 and https://eips.ethereum.org/EIPS/eip-713
///  @param asset The address of the underlying asset to supply
///  @param amount The amount to be supplied
///  @param onBehalfOf The address that will receive the aTokens, same as msg.sender if the user
///    wants to receive them on his own wallet, or a different address if the beneficiary of aTokens
///    is a different wallet
///  @param deadline The deadline timestamp that the permit is valid
///  @param referralCode Code used to register the integrator originating the operation, for potential rewards.
///    0 if the action is executed directly by the user, without any middle-man
///  @param permitV The V parameter of ERC712 permit sig
///  @param permitR The R parameter of ERC712 permit sig
///  @param permitS The S parameter of ERC712 permit sig
function supplyWithPermit(address asset, uint256 amount, address onBehalfOf, uint16 referralCode, uint256 deadline, uint8 permitV, bytes32 permitR, bytes32 permitS) external;;
```

### withdraw(address,uint256,address)

- **Signature**: `withdraw(address,uint256,address)`
- **Visibility**: external
- **Source Range**: 11654:88:4

**Signature:**
```solidity
///  @notice Withdraws an `amount` of underlying asset from the reserve, burning the equivalent aTokens owned
///  E.g. User has 100 aUSDC, calls withdraw() and receives 100 USDC, burning the 100 aUSDC
///  @param asset The address of the underlying asset to withdraw
///  @param amount The underlying amount to be withdrawn
///    - Send the value type(uint256).max in order to withdraw the whole aToken balance
///  @param to The address that will receive the underlying, same as msg.sender if the user
///    wants to receive it on his own wallet, or a different address if the beneficiary is a
///    different wallet
///  @return The final amount withdrawn
function withdraw(address asset, uint256 amount, address to) external returns (uint256);;
```

### borrow(address,uint256,uint256,uint16,address)

- **Signature**: `borrow(address,uint256,uint256,uint16,address)`
- **Visibility**: external
- **Source Range**: 12807:147:4

**Signature:**
```solidity
///  @notice Allows users to borrow a specific `amount` of the reserve underlying asset, provided that the borrower
///  already supplied enough collateral, or he was given enough allowance by a credit delegator on the VariableDebtToken
///  - E.g. User borrows 100 USDC passing as `onBehalfOf` his own address, receiving the 100 USDC in his wallet
///    and 100 variable debt tokens
///  @param asset The address of the underlying asset to borrow
///  @param amount The amount to be borrowed
///  @param interestRateMode 2 for Variable, 1 is deprecated on v3.2.0
///  @param referralCode The code used to register the integrator originating the operation, for potential rewards.
///    0 if the action is executed directly by the user, without any middle-man
///  @param onBehalfOf The address of the user who will receive the debt. Should be the address of the borrower itself
///  calling the function if he wants to borrow against his own collateral, or the address of the credit delegator
///  if he has been given credit delegation allowance
function borrow(address asset, uint256 amount, uint256 interestRateMode, uint16 referralCode, address onBehalfOf) external;;
```

### repay(address,uint256,uint256,address)

- **Signature**: `repay(address,uint256,uint256,address)`
- **Visibility**: external
- **Source Range**: 13777:139:4

**Signature:**
```solidity
///  @notice Repays a borrowed `amount` on a specific reserve, burning the equivalent debt tokens owned
///  - E.g. User repays 100 USDC, burning 100 variable debt tokens of the `onBehalfOf` address
///  @param asset The address of the borrowed underlying asset previously borrowed
///  @param amount The amount to repay
///  - Send the value type(uint256).max in order to repay the whole debt for `asset` on the specific `debtMode`
///  @param interestRateMode 2 for Variable, 1 is deprecated on v3.2.0
///  @param onBehalfOf The address of the user who will get his debt reduced/removed. Should be the address of the
///  user calling the function if he wants to reduce/remove his own debt, or the address of any other
///  other borrower whose debt should be removed
///  @return The final amount repaid
function repay(address asset, uint256 amount, uint256 interestRateMode, address onBehalfOf) external returns (uint256);;
```

### repayWithPermit(address,uint256,uint256,address,uint256,uint8,bytes32,bytes32)

- **Signature**: `repayWithPermit(address,uint256,uint256,address,uint256,uint8,bytes32,bytes32)`
- **Visibility**: external
- **Source Range**: 14958:232:4

**Signature:**
```solidity
///  @notice Repay with transfer approval of asset to be repaid done via permit function
///  see: https://eips.ethereum.org/EIPS/eip-2612 and https://eips.ethereum.org/EIPS/eip-713
///  @param asset The address of the borrowed underlying asset previously borrowed
///  @param amount The amount to repay
///  - Send the value type(uint256).max in order to repay the whole debt for `asset` on the specific `debtMode`
///  @param interestRateMode 2 for Variable, 1 is deprecated on v3.2.0
///  @param onBehalfOf Address of the user who will get his debt reduced/removed. Should be the address of the
///  user calling the function if he wants to reduce/remove his own debt, or the address of any other
///  other borrower whose debt should be removed
///  @param deadline The deadline timestamp that the permit is valid
///  @param permitV The V parameter of ERC712 permit sig
///  @param permitR The R parameter of ERC712 permit sig
///  @param permitS The S parameter of ERC712 permit sig
///  @return The final amount repaid
function repayWithPermit(address asset, uint256 amount, uint256 interestRateMode, address onBehalfOf, uint256 deadline, uint8 permitV, bytes32 permitR, bytes32 permitS) external returns (uint256);;
```

### repayWithATokens(address,uint256,uint256)

- **Signature**: `repayWithATokens(address,uint256,uint256)`
- **Visibility**: external
- **Source Range**: 15898:126:4

**Signature:**
```solidity
///  @notice Repays a borrowed `amount` on a specific reserve using the reserve aTokens, burning the
///  equivalent debt tokens
///  - E.g. User repays 100 USDC using 100 aUSDC, burning 100 variable debt tokens
///  @dev  Passing uint256.max as amount will clean up any residual aToken dust balance, if the user aToken
///  balance is not enough to cover the whole debt
///  @param asset The address of the borrowed underlying asset previously borrowed
///  @param amount The amount to repay
///  - Send the value type(uint256).max in order to repay the whole debt for `asset` on the specific `debtMode`
///  @param interestRateMode DEPRECATED in v3.2.0
///  @return The final amount repaid
function repayWithATokens(address asset, uint256 amount, uint256 interestRateMode) external returns (uint256);;
```

### setUserUseReserveAsCollateral(address,bool)

- **Signature**: `setUserUseReserveAsCollateral(address,bool)`
- **Visibility**: external
- **Source Range**: 16291:85:4

**Signature:**
```solidity
///  @notice Allows suppliers to enable/disable a specific supplied asset as collateral
///  @param asset The address of the underlying asset supplied
///  @param useAsCollateral True if the user wants to use the supply as collateral, false otherwise
function setUserUseReserveAsCollateral(address asset, bool useAsCollateral) external;;
```

### liquidationCall(address,address,address,uint256,bool)

- **Signature**: `liquidationCall(address,address,address,uint256,bool)`
- **Visibility**: external
- **Source Range**: 17243:157:4

**Signature:**
```solidity
///  @notice Function to liquidate a non-healthy position collateral-wise, with Health Factor below 1
///  - The caller (liquidator) covers `debtToCover` amount of debt of the user getting liquidated, and receives
///    a proportionally amount of the `collateralAsset` plus a bonus to cover market risk
///  @param collateralAsset The address of the underlying asset used as collateral, to receive as result of the liquidation
///  @param debtAsset The address of the underlying borrowed asset to be repaid with the liquidation
///  @param user The address of the borrower getting liquidated
///  @param debtToCover The debt amount of borrowed `asset` the liquidator wants to cover
///  @param receiveAToken True if the liquidators wants to receive the collateral aTokens, `false` if he wants
///  to receive the underlying collateral asset directly
function liquidationCall(address collateralAsset, address debtAsset, address user, uint256 debtToCover, bool receiveAToken) external;;
```

### flashLoan(address,address[],uint256[],uint256[],address,bytes,uint16)

- **Signature**: `flashLoan(address,address[],uint256[],uint256[],address,bytes,uint16)`
- **Visibility**: external
- **Source Range**: 18734:242:4

**Signature:**
```solidity
///  @notice Allows smartcontracts to access the liquidity of the pool within one transaction,
///  as long as the amount taken plus a fee is returned.
///  @dev IMPORTANT There are security concerns for developers of flashloan receiver contracts that must be kept
///  into consideration. For further details please visit https://docs.aave.com/developers/
///  @param receiverAddress The address of the contract receiving the funds, implementing IFlashLoanReceiver interface
///  @param assets The addresses of the assets being flash-borrowed
///  @param amounts The amounts of the assets being flash-borrowed
///  @param interestRateModes Types of the debt to open if the flash loan is not returned:
///    0 -> Don't open any debt, just revert if funds can't be transferred from the receiver
///    1 -> Deprecated on v3.2.0
///    2 -> Open debt at variable rate for the value of the amount flash-borrowed to the `onBehalfOf` address
///  @param onBehalfOf The address  that will receive the debt in the case of using 2 on `modes`
///  @param params Variadic packed params to pass to the receiver as extra information
///  @param referralCode The code used to register the integrator originating the operation, for potential rewards.
///    0 if the action is executed directly by the user, without any middle-man
function flashLoan(address receiverAddress, address[] calldata assets, uint256[] calldata amounts, uint256[] calldata interestRateModes, address onBehalfOf, bytes calldata params, uint16 referralCode) external;;
```

### flashLoanSimple(address,address,uint256,bytes,uint16)

- **Signature**: `flashLoanSimple(address,address,uint256,bytes,uint16)`
- **Visibility**: external
- **Source Range**: 19885:158:4

**Signature:**
```solidity
///  @notice Allows smartcontracts to access the liquidity of the pool within one transaction,
///  as long as the amount taken plus a fee is returned.
///  @dev IMPORTANT There are security concerns for developers of flashloan receiver contracts that must be kept
///  into consideration. For further details please visit https://docs.aave.com/developers/
///  @param receiverAddress The address of the contract receiving the funds, implementing IFlashLoanSimpleReceiver interface
///  @param asset The address of the asset being flash-borrowed
///  @param amount The amount of the asset being flash-borrowed
///  @param params Variadic packed params to pass to the receiver as extra information
///  @param referralCode The code used to register the integrator originating the operation, for potential rewards.
///    0 if the action is executed directly by the user, without any middle-man
function flashLoanSimple(address receiverAddress, address asset, uint256 amount, bytes calldata params, uint16 referralCode) external;;
```

### getUserAccountData(address)

- **Signature**: `getUserAccountData(address)`
- **Visibility**: external
- **Source Range**: 20680:281:4

**Signature:**
```solidity
///  @notice Returns the user account data across all the reserves
///  @param user The address of the user
///  @return totalCollateralBase The total collateral of the user in the base currency used by the price feed
///  @return totalDebtBase The total debt of the user in the base currency used by the price feed
///  @return availableBorrowsBase The borrowing power left of the user in the base currency used by the price feed
///  @return currentLiquidationThreshold The liquidation threshold of the user
///  @return ltv The loan to value of The user
///  @return healthFactor The current health factor of the user
function getUserAccountData(address user) external view returns (uint256 totalCollateralBase, uint256 totalDebtBase, uint256 availableBorrowsBase, uint256 currentLiquidationThreshold, uint256 ltv, uint256 healthFactor);;
```

### initReserve(address,address,address,address)

- **Signature**: `initReserve(address,address,address,address)`
- **Visibility**: external
- **Source Range**: 21511:154:4

**Signature:**
```solidity
///  @notice Initializes a reserve, activating it, assigning an aToken and debt tokens and an
///  interest rate strategy
///  @dev Only callable by the PoolConfigurator contract
///  @param asset The address of the underlying asset of the reserve
///  @param aTokenAddress The address of the aToken that will be assigned to the reserve
///  @param variableDebtAddress The address of the VariableDebtToken that will be assigned to the reserve
///  @param interestRateStrategyAddress The address of the interest rate strategy contract
function initReserve(address asset, address aTokenAddress, address variableDebtAddress, address interestRateStrategyAddress) external;;
```

### dropReserve(address)

- **Signature**: `dropReserve(address)`
- **Visibility**: external
- **Source Range**: 21956:45:4

**Signature:**
```solidity
///  @notice Drop a reserve
///  @dev Only callable by the PoolConfigurator contract
///  @dev Does not reset eMode flags, which must be considered when reusing the same reserve id for a different reserve.
///  @param asset The address of the underlying asset of the reserve
function dropReserve(address asset) external;;
```

### setReserveInterestRateStrategyAddress(address,address)

- **Signature**: `setReserveInterestRateStrategyAddress(address,address)`
- **Visibility**: external
- **Source Range**: 22298:112:4

**Signature:**
```solidity
///  @notice Updates the address of the interest rate strategy contract
///  @dev Only callable by the PoolConfigurator contract
///  @param asset The address of the underlying asset of the reserve
///  @param rateStrategyAddress The address of the interest rate strategy contract
function setReserveInterestRateStrategyAddress(address asset, address rateStrategyAddress) external;;
```

### syncIndexesState(address)

- **Signature**: `syncIndexesState(address)`
- **Visibility**: external
- **Source Range**: 22727:50:4

**Signature:**
```solidity
///  @notice Accumulates interest to all indexes of the reserve
///  @dev Only callable by the PoolConfigurator contract
///  @dev To be used when required by the configurator, for example when updating interest rates strategy data
///  @param asset The address of the underlying asset of the reserve
function syncIndexesState(address asset) external;;
```

### syncRatesState(address)

- **Signature**: `syncRatesState(address)`
- **Visibility**: external
- **Source Range**: 23086:48:4

**Signature:**
```solidity
///  @notice Updates interest rates on the reserve data
///  @dev Only callable by the PoolConfigurator contract
///  @dev To be used when required by the configurator, for example when updating interest rates strategy data
///  @param asset The address of the underlying asset of the reserve
function syncRatesState(address asset) external;;
```

### setConfiguration(address,struct DataTypes.ReserveConfigurationMap)

- **Signature**: `setConfiguration(address,struct DataTypes.ReserveConfigurationMap)`
- **Visibility**: external
- **Source Range**: 23400:120:4

**Signature:**
```solidity
///  @notice Sets the configuration bitmap of the reserve as a whole
///  @dev Only callable by the PoolConfigurator contract
///  @param asset The address of the underlying asset of the reserve
///  @param configuration The new configuration bitmap
function setConfiguration(address asset, DataTypes.ReserveConfigurationMap calldata configuration) external;;
```

### getConfiguration(address)

- **Signature**: `getConfiguration(address)`
- **Visibility**: external
- **Source Range**: 23705:114:4

**Signature:**
```solidity
///  @notice Returns the configuration of the reserve
///  @param asset The address of the underlying asset of the reserve
///  @return The configuration of the reserve
function getConfiguration(address asset) external view returns (DataTypes.ReserveConfigurationMap memory);;
```

### getUserConfiguration(address)

- **Signature**: `getUserConfiguration(address)`
- **Visibility**: external
- **Source Range**: 23987:114:4

**Signature:**
```solidity
///  @notice Returns the configuration of the user across all the reserves
///  @param user The user address
///  @return The configuration of the user
function getUserConfiguration(address user) external view returns (DataTypes.UserConfigurationMap memory);;
```

### getReserveNormalizedIncome(address)

- **Signature**: `getReserveNormalizedIncome(address)`
- **Visibility**: external
- **Source Range**: 24289:83:4

**Signature:**
```solidity
///  @notice Returns the normalized income of the reserve
///  @param asset The address of the underlying asset of the reserve
///  @return The reserve's normalized income
function getReserveNormalizedIncome(address asset) external view returns (uint256);;
```

### getReserveNormalizedVariableDebt(address)

- **Signature**: `getReserveNormalizedVariableDebt(address)`
- **Visibility**: external
- **Source Range**: 25184:89:4

**Signature:**
```solidity
///  @notice Returns the normalized variable debt per unit of asset
///  @dev WARNING: This function is intended to be used primarily by the protocol itself to get a
///  "dynamic" variable index based on time, current stored index and virtual rate at the current
///  moment (approx. a borrower would get if opening a position). This means that is always used in
///  combination with variable debt supply/balances.
///  If using this function externally, consider that is possible to have an increasing normalized
///  variable debt that is not equivalent to how the variable debt index would be updated in storage
///  (e.g. only updates with non-zero variable debt supply)
///  @param asset The address of the underlying asset of the reserve
///  @return The reserve normalized variable debt
function getReserveNormalizedVariableDebt(address asset) external view returns (uint256);;
```

### getReserveData(address)

- **Signature**: `getReserveData(address)`
- **Visibility**: external
- **Source Range**: 25483:98:4

**Signature:**
```solidity
///  @notice Returns the state and configuration of the reserve
///  @param asset The address of the underlying asset of the reserve
///  @return The state and configuration data of the reserve
function getReserveData(address asset) external view returns (DataTypes.ReserveDataLegacy memory);;
```

### getVirtualUnderlyingBalance(address)

- **Signature**: `getVirtualUnderlyingBalance(address)`
- **Visibility**: external
- **Source Range**: 25785:84:4

**Signature:**
```solidity
///  @notice Returns the virtual underlying balance of the reserve
///  @param asset The address of the underlying asset of the reserve
///  @return The reserve virtual underlying balance
function getVirtualUnderlyingBalance(address asset) external view returns (uint128);;
```

### finalizeTransfer(address,address,address,uint256,uint256,uint256)

- **Signature**: `finalizeTransfer(address,address,address,uint256,uint256,uint256)`
- **Visibility**: external
- **Source Range**: 26413:172:4

**Signature:**
```solidity
///  @notice Validates and finalizes an aToken transfer
///  @dev Only callable by the overlying aToken of the `asset`
///  @param asset The address of the underlying asset of the aToken
///  @param from The user from which the aTokens are transferred
///  @param to The user receiving the aTokens
///  @param amount The amount being transferred/withdrawn
///  @param balanceFromBefore The aToken balance of the `from` user before the transfer
///  @param balanceToBefore The aToken balance of the `to` user before the transfer
function finalizeTransfer(address asset, address from, address to, uint256 amount, uint256 balanceFromBefore, uint256 balanceToBefore) external;;
```

### getReservesList()

- **Signature**: `getReservesList()`
- **Visibility**: external
- **Source Range**: 26815:68:4

**Signature:**
```solidity
///  @notice Returns the list of the underlying assets of all the initialized reserves
///  @dev It does not include dropped reserves
///  @return The addresses of the underlying assets of the initialized reserves
function getReservesList() external view returns (address[] memory);;
```

### getReservesCount()

- **Signature**: `getReservesCount()`
- **Visibility**: external
- **Source Range**: 27017:60:4

**Signature:**
```solidity
///  @notice Returns the number of initialized reserves
///  @dev It includes dropped reserves
///  @return The count
function getReservesCount() external view returns (uint256);;
```

### getReserveAddressById(uint16)

- **Signature**: `getReserveAddressById(uint16)`
- **Visibility**: external
- **Source Range**: 27369:74:4

**Signature:**
```solidity
///  @notice Returns the address of the underlying asset of a reserve by the reserve id as stored in the DataTypes.ReserveData struct
///  @param id The id of the reserve as stored in the DataTypes.ReserveData struct
///  @return The address of the reserve associated with id
function getReserveAddressById(uint16 id) external view returns (address);;
```

### ADDRESSES_PROVIDER()

- **Signature**: `ADDRESSES_PROVIDER()`
- **Visibility**: external
- **Source Range**: 27587:77:4

**Signature:**
```solidity
///  @notice Returns the PoolAddressesProvider connected to this contract
///  @return The address of the PoolAddressesProvider
function ADDRESSES_PROVIDER() external view returns (IPoolAddressesProvider);;
```

### updateBridgeProtocolFee(uint256)

- **Signature**: `updateBridgeProtocolFee(uint256)`
- **Visibility**: external
- **Source Range**: 27818:69:4

**Signature:**
```solidity
///  @notice Updates the protocol fee on the bridging
///  @param bridgeProtocolFee The part of the premium sent to the protocol treasury
function updateBridgeProtocolFee(uint256 bridgeProtocolFee) external;;
```

### updateFlashloanPremiums(uint128,uint128)

- **Signature**: `updateFlashloanPremiums(uint128,uint128)`
- **Visibility**: external
- **Source Range**: 28544:121:4

**Signature:**
```solidity
///  @notice Updates flash loan premiums. Flash loan premium consists of two parts:
///  - A part is sent to aToken holders as extra, one time accumulated interest
///  - A part is collected by the protocol treasury
///  @dev The total premium is calculated on the total borrowed amount
///  @dev The premium to protocol is calculated on the total premium, being a percentage of `flashLoanPremiumTotal`
///  @dev Only callable by the PoolConfigurator contract
///  @param flashLoanPremiumTotal The total premium, expressed in bps
///  @param flashLoanPremiumToProtocol The part of the premium sent to the protocol treasury, expressed in bps
function updateFlashloanPremiums(uint128 flashLoanPremiumTotal, uint128 flashLoanPremiumToProtocol) external;;
```

### configureEModeCategory(uint8,struct DataTypes.EModeCategoryBaseConfiguration)

- **Signature**: `configureEModeCategory(uint8,struct DataTypes.EModeCategoryBaseConfiguration)`
- **Visibility**: external
- **Source Range**: 29039:119:4

**Signature:**
```solidity
///  @notice Configures a new or alters an existing collateral configuration of an eMode.
///  @dev In eMode, the protocol allows very high borrowing power to borrow assets of the same category.
///  The category 0 is reserved as it's the default for volatile assets
///  @param id The id of the category
///  @param config The configuration of the category
function configureEModeCategory(uint8 id, DataTypes.EModeCategoryBaseConfiguration memory config) external;;
```

### configureEModeCategoryCollateralBitmap(uint8,uint128)

- **Signature**: `configureEModeCategoryCollateralBitmap(uint8,uint128)`
- **Visibility**: external
- **Source Range**: 29336:93:4

**Signature:**
```solidity
///  @notice Replaces the current eMode collateralBitmap.
///  @param id The id of the category
///  @param collateralBitmap The collateralBitmap of the category
function configureEModeCategoryCollateralBitmap(uint8 id, uint128 collateralBitmap) external;;
```

### configureEModeCategoryBorrowableBitmap(uint8,uint128)

- **Signature**: `configureEModeCategoryBorrowableBitmap(uint8,uint128)`
- **Visibility**: external
- **Source Range**: 29607:93:4

**Signature:**
```solidity
///  @notice Replaces the current eMode borrowableBitmap.
///  @param id The id of the category
///  @param borrowableBitmap The borrowableBitmap of the category
function configureEModeCategoryBorrowableBitmap(uint8 id, uint128 borrowableBitmap) external;;
```

### getEModeCategoryData(uint8)

- **Signature**: `getEModeCategoryData(uint8)`
- **Visibility**: external
- **Source Range**: 29910:109:4

**Signature:**
```solidity
///  @notice Returns the data of an eMode category
///  @dev DEPRECATED use independent getters instead
///  @param id The id of the category
///  @return The configuration data of the category
function getEModeCategoryData(uint8 id) external view returns (DataTypes.EModeCategoryLegacy memory);;
```

### getEModeCategoryLabel(uint8)

- **Signature**: `getEModeCategoryLabel(uint8)`
- **Visibility**: external
- **Source Range**: 30164:79:4

**Signature:**
```solidity
///  @notice Returns the label of an eMode category
///  @param id The id of the category
///  @return The label of the category
function getEModeCategoryLabel(uint8 id) external view returns (string memory);;
```

### getEModeCategoryCollateralConfig(uint8)

- **Signature**: `getEModeCategoryCollateralConfig(uint8)`
- **Visibility**: external
- **Source Range**: 30404:118:4

**Signature:**
```solidity
///  @notice Returns the collateral config of an eMode category
///  @param id The id of the category
///  @return The ltv,lt,lb of the category
function getEModeCategoryCollateralConfig(uint8 id) external view returns (DataTypes.CollateralConfig memory);;
```

### getEModeCategoryCollateralBitmap(uint8)

- **Signature**: `getEModeCategoryCollateralBitmap(uint8)`
- **Visibility**: external
- **Source Range**: 30689:84:4

**Signature:**
```solidity
///  @notice Returns the collateralBitmap of an eMode category
///  @param id The id of the category
///  @return The collateralBitmap of the category
function getEModeCategoryCollateralBitmap(uint8 id) external view returns (uint128);;
```

### getEModeCategoryBorrowableBitmap(uint8)

- **Signature**: `getEModeCategoryBorrowableBitmap(uint8)`
- **Visibility**: external
- **Source Range**: 30940:84:4

**Signature:**
```solidity
///  @notice Returns the borrowableBitmap of an eMode category
///  @param id The id of the category
///  @return The borrowableBitmap of the category
function getEModeCategoryBorrowableBitmap(uint8 id) external view returns (uint128);;
```

### setUserEMode(uint8)

- **Signature**: `setUserEMode(uint8)`
- **Visibility**: external
- **Source Range**: 31142:49:4

**Signature:**
```solidity
///  @notice Allows a user to use the protocol in eMode
///  @param categoryId The id of the category
function setUserEMode(uint8 categoryId) external;;
```

### getUserEMode(address)

- **Signature**: `getUserEMode(address)`
- **Visibility**: external
- **Source Range**: 31323:68:4

**Signature:**
```solidity
///  @notice Returns the eMode the user is using
///  @param user The address of the user
///  @return The eMode id
function getUserEMode(address user) external view returns (uint256);;
```

### resetIsolationModeTotalDebt(address)

- **Signature**: `resetIsolationModeTotalDebt(address)`
- **Visibility**: external
- **Source Range**: 31634:61:4

**Signature:**
```solidity
///  @notice Resets the isolation mode total debt of the given asset to zero
///  @dev It requires the given asset has zero debt ceiling
///  @param asset The address of the underlying asset to reset the isolationModeTotalDebt
function resetIsolationModeTotalDebt(address asset) external;;
```

### setLiquidationGracePeriod(address,uint40)

- **Signature**: `setLiquidationGracePeriod(address,uint40)`
- **Visibility**: external
- **Source Range**: 32115:73:4

**Signature:**
```solidity
///  @notice Sets the liquidation grace period of the given asset
///  @dev To enable a liquidation grace period, a timestamp in the future should be set,
///       To disable a liquidation grace period, any timestamp in the past works, like 0
///  @param asset The address of the underlying asset to set the liquidationGracePeriod
///  @param until Timestamp when the liquidation grace period will end*
function setLiquidationGracePeriod(address asset, uint40 until) external;;
```

### getLiquidationGracePeriod(address)

- **Signature**: `getLiquidationGracePeriod(address)`
- **Visibility**: external
- **Source Range**: 32394:81:4

**Signature:**
```solidity
///  @notice Returns the liquidation grace period of the given asset
///  @param asset The address of the underlying asset
///  @return Timestamp when the liquidation grace period will end*
function getLiquidationGracePeriod(address asset) external view returns (uint40);;
```

### FLASHLOAN_PREMIUM_TOTAL()

- **Signature**: `FLASHLOAN_PREMIUM_TOTAL()`
- **Visibility**: external
- **Source Range**: 32582:67:4

**Signature:**
```solidity
///  @notice Returns the total fee on flash loans
///  @return The total fee on flashloans
function FLASHLOAN_PREMIUM_TOTAL() external view returns (uint128);;
```

### BRIDGE_PROTOCOL_FEE()

- **Signature**: `BRIDGE_PROTOCOL_FEE()`
- **Visibility**: external
- **Source Range**: 32789:63:4

**Signature:**
```solidity
///  @notice Returns the part of the bridge fees sent to protocol
///  @return The bridge fee sent to the protocol treasury
function BRIDGE_PROTOCOL_FEE() external view returns (uint256);;
```

### FLASHLOAN_PREMIUM_TO_PROTOCOL()

- **Signature**: `FLASHLOAN_PREMIUM_TO_PROTOCOL()`
- **Visibility**: external
- **Source Range**: 32998:73:4

**Signature:**
```solidity
///  @notice Returns the part of the flashloan fees sent to protocol
///  @return The flashloan fee sent to the protocol treasury
function FLASHLOAN_PREMIUM_TO_PROTOCOL() external view returns (uint128);;
```

### MAX_NUMBER_RESERVES()

- **Signature**: `MAX_NUMBER_RESERVES()`
- **Visibility**: external
- **Source Range**: 33229:62:4

**Signature:**
```solidity
///  @notice Returns the maximum number of reserves supported to be listed in this Pool
///  @return The maximum number of reserves supported
function MAX_NUMBER_RESERVES() external view returns (uint16);;
```

### mintToTreasury(address[])

- **Signature**: `mintToTreasury(address[])`
- **Visibility**: external
- **Source Range**: 33494:60:4

**Signature:**
```solidity
///  @notice Mints the assets accrued through the reserve factor to the treasury in the form of aTokens
///  @param assets The list of reserves for which the minting needs to be executed
function mintToTreasury(address[] calldata assets) external;;
```

### rescueTokens(address,address,uint256)

- **Signature**: `rescueTokens(address,address,uint256)`
- **Visibility**: external
- **Source Range**: 33772:74:4

**Signature:**
```solidity
///  @notice Rescue and transfer tokens locked in this contract
///  @param token The address of the token
///  @param to The address of the recipient
///  @param amount The amount of token to transfer
function rescueTokens(address token, address to, uint256 amount) external;;
```

### deposit(address,uint256,address,uint16)

- **Signature**: `deposit(address,uint256,address,uint16)`
- **Visibility**: external
- **Source Range**: 34621:98:4

**Signature:**
```solidity
///  @notice Supplies an `amount` of underlying asset into the reserve, receiving in return overlying aTokens.
///  - E.g. User supplies 100 USDC and gets in return 100 aUSDC
///  @dev Deprecated: Use the `supply` function instead
///  @param asset The address of the underlying asset to supply
///  @param amount The amount to be supplied
///  @param onBehalfOf The address that will receive the aTokens, same as msg.sender if the user
///    wants to receive them on his own wallet, or a different address if the beneficiary of aTokens
///    is a different wallet
///  @param referralCode Code used to register the integrator originating the operation, for potential rewards.
///    0 if the action is executed directly by the user, without any middle-man
function deposit(address asset, uint256 amount, address onBehalfOf, uint16 referralCode) external;;
```

### eliminateReserveDeficit(address,uint256)

- **Signature**: `eliminateReserveDeficit(address,uint256)`
- **Visibility**: external
- **Source Range**: 35285:73:4

**Signature:**
```solidity
///  @notice It covers the deficit of a specified reserve by burning:
///  - the equivalent aToken `amount` for assets with virtual accounting enabled
///  - the equivalent `amount` of underlying for assets with virtual accounting disabled (e.g. GHO)
///  @dev The deficit of a reserve can occur due to situations where borrowed assets are not repaid, leading to bad debt.
///  @param asset The address of the underlying asset to cover the deficit.
///  @param amount The amount to be covered, in aToken or underlying on non-virtual accounted assets
function eliminateReserveDeficit(address asset, uint256 amount) external;;
```

### getReserveDeficit(address)

- **Signature**: `getReserveDeficit(address)`
- **Visibility**: external
- **Source Range**: 35546:74:4

**Signature:**
```solidity
///  @notice Returns the current deficit of a reserve.
///  @param asset The address of the underlying asset of the reserve
///  @return The current deficit of the reserve
function getReserveDeficit(address asset) external view returns (uint256);;
```

### getReserveAToken(address)

- **Signature**: `getReserveAToken(address)`
- **Visibility**: external
- **Source Range**: 35798:73:4

**Signature:**
```solidity
///  @notice Returns the aToken address of a reserve.
///  @param asset The address of the underlying asset of the reserve
///  @return The address of the aToken
function getReserveAToken(address asset) external view returns (address);;
```

### getReserveVariableDebtToken(address)

- **Signature**: `getReserveVariableDebtToken(address)`
- **Visibility**: external
- **Source Range**: 36071:84:4

**Signature:**
```solidity
///  @notice Returns the variableDebtToken address of a reserve.
///  @param asset The address of the underlying asset of the reserve
///  @return The address of the variableDebtToken
function getReserveVariableDebtToken(address asset) external view returns (address);;
```

### getFlashLoanLogic()

- **Signature**: `getFlashLoanLogic()`
- **Visibility**: external
- **Source Range**: 36232:61:4

**Signature:**
```solidity
///  @notice Gets the address of the external FlashLoanLogic
function getFlashLoanLogic() external view returns (address);;
```

### getBorrowLogic()

- **Signature**: `getBorrowLogic()`
- **Visibility**: external
- **Source Range**: 36367:58:4

**Signature:**
```solidity
///  @notice Gets the address of the external BorrowLogic
function getBorrowLogic() external view returns (address);;
```

### getBridgeLogic()

- **Signature**: `getBridgeLogic()`
- **Visibility**: external
- **Source Range**: 36499:58:4

**Signature:**
```solidity
///  @notice Gets the address of the external BridgeLogic
function getBridgeLogic() external view returns (address);;
```

### getEModeLogic()

- **Signature**: `getEModeLogic()`
- **Visibility**: external
- **Source Range**: 36630:57:4

**Signature:**
```solidity
///  @notice Gets the address of the external EModeLogic
function getEModeLogic() external view returns (address);;
```

### getLiquidationLogic()

- **Signature**: `getLiquidationLogic()`
- **Visibility**: external
- **Source Range**: 36766:63:4

**Signature:**
```solidity
///  @notice Gets the address of the external LiquidationLogic
function getLiquidationLogic() external view returns (address);;
```

### getPoolLogic()

- **Signature**: `getPoolLogic()`
- **Visibility**: external
- **Source Range**: 36901:56:4

**Signature:**
```solidity
///  @notice Gets the address of the external PoolLogic
function getPoolLogic() external view returns (address);;
```

### getSupplyLogic()

- **Signature**: `getSupplyLogic()`
- **Visibility**: external
- **Source Range**: 37031:58:4

**Signature:**
```solidity
///  @notice Gets the address of the external SupplyLogic
function getSupplyLogic() external view returns (address);;
```
