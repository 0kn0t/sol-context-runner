# Interface: IInitializableAToken

## Metadata

- **Name**: IInitializableAToken
- **Type**: Interface
- **Path**: lib/aave-v3-origin/src/contracts/interfaces/IInitializableAToken.sol
- **Documentation**:  @title IInitializableAToken
   @author Aave
   @notice Interface for the initialize function on AToken

## Events

### Initialized

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

## Public/External Functions

### initialize(contract IPool,address,address,contract IAaveIncentivesController,uint8,string,string,bytes)

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
