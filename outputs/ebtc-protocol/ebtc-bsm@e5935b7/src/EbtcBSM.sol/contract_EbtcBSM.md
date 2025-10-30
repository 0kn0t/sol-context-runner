# Contract: EbtcBSM

## Metadata

- **Name**: EbtcBSM
- **Type**: Contract
- **Path**: src/EbtcBSM.sol
- **Documentation**:  @title eBTC Stability Module (BSM) Contract
   @notice Facilitates bi-directional exchange between eBTC and other BTC-denominated assets with no slippage.
   @dev This contract handles the core business logic for asset token operations including minting and redeeming eBTC.

## Implements Interfaces

- **IEbtcBSM** [src/Dependencies/IEbtcBSM.sol/interface_IEbtcBSM.md]

## State Variables

### _paused (inherited from Pausable)

```solidity
bool private _paused
```

### INITIALIZABLE_STORAGE (inherited from Initializable)

```solidity
bytes32 private constant INITIALIZABLE_STORAGE = 0xf0c57e16840df040f15088dc2f81fe391c3923bec73e23a9662efc9c229c6a00
```

### _authority (inherited from AuthNoOwner)

```solidity
Authority private _authority
```

**Authority**: [src/Dependencies/Authority.sol/interface_Authority.md]

### _authorityInitialized (inherited from AuthNoOwner)

```solidity
bool private _authorityInitialized
```

### ASSET_TOKEN_PRECISION

```solidity
uint256 public immutable ASSET_TOKEN_PRECISION
```

### BPS

```solidity
/// @notice Basis points constant for percentage calculations
uint256 public constant BPS = 10_000
```

### MAX_FEE

```solidity
/// @notice Maximum allowable fees in basis points
uint256 public constant MAX_FEE = 2_000
```

### ASSET_TOKEN

```solidity
/// @notice Underlying asset token for eBTC
IERC20 public immutable ASSET_TOKEN
```

**IERC20**: [lib/openzeppelin-contracts/contracts/token/ERC20/IERC20.sol/interface_IERC20.md]

### EBTC_TOKEN

```solidity
/// @notice eBTC token contract
IEbtcToken public immutable EBTC_TOKEN
```

**IEbtcToken**: [src/Dependencies/IEbtcToken.sol/interface_IEbtcToken.md]

### feeToSellBPS

```solidity
/// @notice Fee for selling assets into eBTC (in basis points)
uint256 public feeToSellBPS
```

### feeToBuyBPS

```solidity
/// @notice Fee for buying assets with eBTC (in basis points)
uint256 public feeToBuyBPS
```

### totalMinted

```solidity
/// @notice Total amount of eBTC minted
uint256 public totalMinted
```

### escrow

```solidity
/// @notice Escrow contract to hold asset tokens
IEscrow public escrow
```

**IEscrow**: [src/Dependencies/IEscrow.sol/interface_IEscrow.md]

### oraclePriceConstraint

```solidity
/// @notice Oracle-based price constraint for minting
IConstraint public oraclePriceConstraint
```

**IConstraint**: [src/Dependencies/IConstraint.sol/interface_IConstraint.md]

### rateLimitingConstraint

```solidity
/// @notice Rate limiting constraint for minting
IConstraint public rateLimitingConstraint
```

**IConstraint**: [src/Dependencies/IConstraint.sol/interface_IConstraint.md]

### buyAssetConstraint

```solidity
/// @notice Constraint for buying asset tokens
IConstraint public buyAssetConstraint
```

**IConstraint**: [src/Dependencies/IConstraint.sol/interface_IConstraint.md]

## Structs

### InitializableStorage (inherited from Initializable)

```solidity
///  @dev Storage of the initializable contract.
///  It's implemented on a custom ERC-7201 namespace to reduce the risk of storage collisions
///  when using with upgradeable contracts.
///  @custom:storage-location erc7201:openzeppelin.storage.Initializable
struct InitializableStorage {
    uint64 _initialized;
    bool _initializing;
}
```

## Errors

### EnforcedPause (inherited from Pausable)

```solidity
///  @dev The operation failed because the contract is paused.
error EnforcedPause();
```

### ExpectedPause (inherited from Pausable)

```solidity
///  @dev The operation failed because the contract is not paused.
error ExpectedPause();
```

### InvalidInitialization (inherited from Initializable)

```solidity
///  @dev The contract is already initialized.
error InvalidInitialization();
```

### NotInitializing (inherited from Initializable)

```solidity
///  @dev The contract is not initializing.
error NotInitializing();
```

### InsufficientAssetTokens

```solidity
/// @notice Error for when there are insufficient asset tokens available
error InsufficientAssetTokens(uint256 required, uint256 available);
```

### BelowExpectedMinOutAmount

```solidity
/// @notice Error for when the actual output amount is below the expected amount
error BelowExpectedMinOutAmount(uint256 expected, uint256 actual);
```

### ZeroAmount

```solidity
/// @notice Error for when the amount passed into sellAsset or buyAsset is zero
error ZeroAmount();
```

### InvalidAddress

```solidity
/// @notice Error for when an address is the zero address
error InvalidAddress();
```

### InvalidFee

```solidity
/// @notice Error for when trying to set an invalid fee
error InvalidFee();
```

## Events

### EscrowUpdated (inherited from IEbtcBSM)

```solidity
event EscrowUpdated(address indexed oldVault, address indexed newVault);
```

### FeeToSellUpdated (inherited from IEbtcBSM)

```solidity
event FeeToSellUpdated(uint256 oldFee, uint256 newFee);
```

### FeeToBuyUpdated (inherited from IEbtcBSM)

```solidity
event FeeToBuyUpdated(uint256 oldFee, uint256 newFee);
```

### AssetSold (inherited from IEbtcBSM)

```solidity
event AssetSold(uint256 assetAmountIn, uint256 ebtcAmountOut, uint256 feeAmount);
```

### AssetBought (inherited from IEbtcBSM)

```solidity
event AssetBought(uint256 ebtcAmountIn, uint256 assetAmountOut, uint256 feeAmount);
```

### Paused (inherited from Pausable)

```solidity
///  @dev Emitted when the pause is triggered by `account`.
event Paused(address account);
```

### Unpaused (inherited from Pausable)

```solidity
///  @dev Emitted when the pause is lifted by `account`.
event Unpaused(address account);
```

### Initialized (inherited from Initializable)

```solidity
///  @dev Triggered when the contract has been initialized or reinitialized.
event Initialized(uint64 version);
```

### AuthorityUpdated (inherited from AuthNoOwner)

```solidity
event AuthorityUpdated(address indexed user, Authority indexed newAuthority);
```

## Public/External Functions

### constructor(address,address,address,address,address,address)

- **Signature**: `constructor(address,address,address,address,address,address)`
- **Visibility**: public
- **Source Range**: 3171:975:40
- **Details**: [function_constructor_address_address_address_address_address_address.md](./function_constructor_address_address_address_address_address_address.md)

**Signature:**
```solidity
/// @notice Constructs the EbtcBSM contract
///  @param _assetToken Address of the underlying asset token
///  @param _oraclePriceConstraint Address of the oracle price constraint
///  @param _rateLimitingConstraint Address of the rate limiting constraint
///  @param _ebtcToken Address of the eBTC token
///  @param _governance Address of the governor
constructor(address _assetToken, address _oraclePriceConstraint, address _rateLimitingConstraint, address _buyAssetConstraint, address _ebtcToken, address _governance);
```

### initialize(address)

- **Signature**: `initialize(address)`
- **Visibility**: external
- **Source Range**: 4401:140:40
- **Details**: [function_initialize_address.md](./function_initialize_address.md)

**Signature:**
```solidity
/// @notice This function will be invoked only once within the same transaction as the deployment of
///  this contract, thereby preventing any other user from executing this function.
///  @param _escrow Address of the escrow contract
function initialize(address _escrow) external initializer();
```

### previewSellAsset(uint256)

- **Signature**: `previewSellAsset(uint256)`
- **Visibility**: external
- **Source Range**: 11770:210:40
- **Details**: [function_previewSellAsset_uint256.md](./function_previewSellAsset_uint256.md)

**Signature:**
```solidity
///  @notice Calculates the amount of eBTC minted for a given amount of asset tokens accounting
///  for all minting constraints
///  @param _assetAmountIn the total amount intended to be deposited
///  @return _ebtcAmountOut the estimated eBTC to mint after fees
function previewSellAsset(uint256 _assetAmountIn) external view whenNotPaused() returns (uint256 _ebtcAmountOut);
```

### previewBuyAsset(uint256)

- **Signature**: `previewBuyAsset(uint256)`
- **Visibility**: external
- **Source Range**: 12236:310:40
- **Details**: [function_previewBuyAsset_uint256.md](./function_previewBuyAsset_uint256.md)

**Signature:**
```solidity
///  @notice Calculates the net asset amount that can be bought with a given amount of eBTC
///  @param _ebtcAmountIn the total amount intended to be deposited
///  @return _assetAmountOut the estimated asset to buy after fees
function previewBuyAsset(uint256 _ebtcAmountIn) external view whenNotPaused() returns (uint256 _assetAmountOut);
```

### previewSellAssetNoFee(uint256)

- **Signature**: `previewSellAssetNoFee(uint256)`
- **Visibility**: external
- **Source Range**: 12850:190:40
- **Details**: [function_previewSellAssetNoFee_uint256.md](./function_previewSellAssetNoFee_uint256.md)

**Signature:**
```solidity
///  @notice Calculates the amount of eBTC minted for a given amount of asset tokens accounting
///  for all minting constraints (no fee)
///  @param _assetAmountIn the total amount intended to be deposited
///  @return _ebtcAmountOut the estimated eBTC to mint after fees
function previewSellAssetNoFee(uint256 _assetAmountIn) external view whenNotPaused() returns (uint256 _ebtcAmountOut);
```

### previewBuyAssetNoFee(uint256)

- **Signature**: `previewBuyAssetNoFee(uint256)`
- **Visibility**: external
- **Source Range**: 13305:206:40
- **Details**: [function_previewBuyAssetNoFee_uint256.md](./function_previewBuyAssetNoFee_uint256.md)

**Signature:**
```solidity
///  @notice Calculates the net asset amount that can be bought with a given amount of eBTC (no fee)
///  @param _ebtcAmountIn the total amount intended to be deposited
///  @return _assetAmountOut the estimated asset to buy after fees
function previewBuyAssetNoFee(uint256 _ebtcAmountIn) external view whenNotPaused() returns (uint256 _assetAmountOut);
```

### sellAsset(uint256,address,uint256)

- **Signature**: `sellAsset(uint256,address,uint256)`
- **Visibility**: external
- **Source Range**: 13862:289:40
- **Details**: [function_sellAsset_uint256_address_uint256.md](./function_sellAsset_uint256_address_uint256.md)

**Signature:**
```solidity
///  @notice Allows users to mint eBTC by depositing asset tokens
///  @param _assetAmountIn Amount of asset tokens to deposit
///  @param _recipient custom recipient for the minted eBTC
///  @param _minOutAmount minimum eBTC expected after slippage
///  @return _ebtcAmountOut Amount of eBTC tokens minted to the user
function sellAsset(uint256 _assetAmountIn, address _recipient, uint256 _minOutAmount) external whenNotPaused() returns (uint256 _ebtcAmountOut);
```

### buyAsset(uint256,address,uint256)

- **Signature**: `buyAsset(uint256,address,uint256)`
- **Visibility**: external
- **Source Range**: 14507:438:40
- **Details**: [function_buyAsset_uint256_address_uint256.md](./function_buyAsset_uint256_address_uint256.md)

**Signature:**
```solidity
///  @notice Allows users to buy BSM owned asset tokens by burning their eBTC
///  @param _ebtcAmountIn Amount of eBTC tokens to burn
///  @param _recipient custom recipient for the asset
///  @param _minOutAmount minimum asset tokens expected after slippage
///  @return _assetAmountOut Amount of asset tokens sent to user
function buyAsset(uint256 _ebtcAmountIn, address _recipient, uint256 _minOutAmount) external whenNotPaused() returns (uint256 _assetAmountOut);
```

### sellAssetNoFee(uint256,address,uint256)

- **Signature**: `sellAssetNoFee(uint256,address,uint256)`
- **Visibility**: external
- **Source Range**: 15381:270:40
- **Details**: [function_sellAssetNoFee_uint256_address_uint256.md](./function_sellAssetNoFee_uint256_address_uint256.md)

**Signature:**
```solidity
///  @notice Allows authorized users to mint eBTC by depositing asset tokens without applying a fee
///  @dev can only be called by authorized users
///  @param _assetAmountIn Amount of asset tokens to deposit
///  @param _recipient custom recipient for the minted eBTC
///  @param _minOutAmount minimum eBTC expected after slippage
///  @return _ebtcAmountOut Amount of eBTC tokens minted to the user
function sellAssetNoFee(uint256 _assetAmountIn, address _recipient, uint256 _minOutAmount) external whenNotPaused() requiresAuth() returns (uint256 _ebtcAmountOut);
```

### buyAssetNoFee(uint256,address,uint256)

- **Signature**: `buyAssetNoFee(uint256,address,uint256)`
- **Visibility**: external
- **Source Range**: 16069:359:40
- **Details**: [function_buyAssetNoFee_uint256_address_uint256.md](./function_buyAssetNoFee_uint256_address_uint256.md)

**Signature:**
```solidity
///  @notice Allows authorized users to buy BSM owned asset tokens by burning their eBTC
///  @dev Can only be called by authorized users
///  @param _ebtcAmountIn Amount of eBTC tokens to burn
///  @param _recipient custom recipient for the asset
///  @param _minOutAmount minimum asset tokens expected after slippage
///  @return _assetAmountOut Amount of asset tokens sent to user
function buyAssetNoFee(uint256 _ebtcAmountIn, address _recipient, uint256 _minOutAmount) external whenNotPaused() requiresAuth() returns (uint256 _assetAmountOut);
```

### setFeeToSell(uint256)

- **Signature**: `setFeeToSell(uint256)`
- **Visibility**: external
- **Source Range**: 16584:229:40
- **Details**: [function_setFeeToSell_uint256.md](./function_setFeeToSell_uint256.md)

**Signature:**
```solidity
/// @notice Sets the fee for selling eBTC
///  @dev Can only be called by authorized users
///  @param _feeToSellBPS Fee in basis points
function setFeeToSell(uint256 _feeToSellBPS) external requiresAuth();
```

### setFeeToBuy(uint256)

- **Signature**: `setFeeToBuy(uint256)`
- **Visibility**: external
- **Source Range**: 16967:221:40
- **Details**: [function_setFeeToBuy_uint256.md](./function_setFeeToBuy_uint256.md)

**Signature:**
```solidity
/// @notice Sets the fee for buying eBTC
///  @dev Can only be called by authorized users
///  @param _feeToBuyBPS Fee in basis points
function setFeeToBuy(uint256 _feeToBuyBPS) external requiresAuth();
```

### setRateLimitingConstraint(address)

- **Signature**: `setRateLimitingConstraint(address)`
- **Visibility**: external
- **Source Range**: 17397:356:40
- **Details**: [function_setRateLimitingConstraint_address.md](./function_setRateLimitingConstraint_address.md)

**Signature:**
```solidity
/// @notice Updates the rate limiting constraint address
///  @dev Can only be called by authorized users
///  @param _newRateLimitingConstraint New address for the rate limiting constraint
function setRateLimitingConstraint(address _newRateLimitingConstraint) external requiresAuth();
```

### setOraclePriceConstraint(address)

- **Signature**: `setOraclePriceConstraint(address)`
- **Visibility**: external
- **Source Range**: 17959:349:40
- **Details**: [function_setOraclePriceConstraint_address.md](./function_setOraclePriceConstraint_address.md)

**Signature:**
```solidity
/// @notice Updates the oracle price constraint address
///  @dev Can only be called by authorized users
///  @param _newOraclePriceConstraint New address for the oracle price constraint
function setOraclePriceConstraint(address _newOraclePriceConstraint) external requiresAuth();
```

### setBuyAssetConstraint(address)

- **Signature**: `setBuyAssetConstraint(address)`
- **Visibility**: external
- **Source Range**: 18505:328:40
- **Details**: [function_setBuyAssetConstraint_address.md](./function_setBuyAssetConstraint_address.md)

**Signature:**
```solidity
/// @notice Updates the buy asset constraint address
///  @dev Can only be called by authorized users
///  @param _newBuyAssetConstraint New address for the buy asset constraint
function setBuyAssetConstraint(address _newBuyAssetConstraint) external requiresAuth();
```

### updateEscrow(address)

- **Signature**: `updateEscrow(address)`
- **Visibility**: external
- **Source Range**: 19016:745:40
- **Details**: [function_updateEscrow_address.md](./function_updateEscrow_address.md)

**Signature:**
```solidity
/// @notice Updates the escrow address and initiates an escrow migration
///  @dev Can only be called by authorized users
///  @param _newEscrow New escrow address
function updateEscrow(address _newEscrow) external requiresAuth();
```

### pause()

- **Signature**: `pause()`
- **Visibility**: external
- **Source Range**: 19866:64:40
- **Details**: [function_pause.md](./function_pause.md)

**Signature:**
```solidity
/// @notice Pauses the contract operations
///  @dev Can only be called by authorized users
function pause() external requiresAuth();
```

### unpause()

- **Signature**: `unpause()`
- **Visibility**: external
- **Source Range**: 20037:68:40
- **Details**: [function_unpause.md](./function_unpause.md)

**Signature:**
```solidity
/// @notice Unpauses the contract operations
///  @dev Can only be called by authorized users
function unpause() external requiresAuth();
```

### paused() (inherited from Pausable)

- **Signature**: `paused()`
- **Visibility**: public
- **Source Range**: 1850:84:15
- **Details**: [function_paused.md](./function_paused.md)

**Signature:**
```solidity
///  @dev Returns true if the contract is paused, and false otherwise.
function paused() virtual public view returns (bool);
```

### authority() (inherited from AuthNoOwner)

- **Signature**: `authority()`
- **Visibility**: public
- **Source Range**: 778:87:25
- **Details**: [function_authority.md](./function_authority.md)

**Signature:**
```solidity
function authority() public view returns (Authority);
```

### authorityInitialized() (inherited from AuthNoOwner)

- **Signature**: `authorityInitialized()`
- **Visibility**: public
- **Source Range**: 871:104:25
- **Details**: [function_authorityInitialized.md](./function_authorityInitialized.md)

**Signature:**
```solidity
function authorityInitialized() public view returns (bool);
```
