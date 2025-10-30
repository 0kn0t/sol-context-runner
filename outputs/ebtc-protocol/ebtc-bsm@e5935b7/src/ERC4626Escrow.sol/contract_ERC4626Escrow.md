# Contract: ERC4626Escrow

## Metadata

- **Name**: ERC4626Escrow
- **Type**: Contract
- **Path**: src/ERC4626Escrow.sol
- **Documentation**: @title ERC4626 Escrow Contract
   @notice This contract extends the BaseEscrow to interact with an ERC4626 compliant external vault for asset management
   and for additional yield opportunities.

## Implements Interfaces

- **IERC4626Escrow** [src/Dependencies/IERC4626Escrow.sol/interface_IERC4626Escrow.md]
- **IEscrow** [src/Dependencies/IEscrow.sol/interface_IEscrow.md]

## State Variables

### _authority (inherited from AuthNoOwner)

```solidity
Authority private _authority
```

**Authority**: [src/Dependencies/Authority.sol/interface_Authority.md]

### _authorityInitialized (inherited from AuthNoOwner)

```solidity
bool private _authorityInitialized
```

### ASSET_TOKEN (inherited from BaseEscrow)

```solidity
IERC20 public immutable ASSET_TOKEN
```

**IERC20**: [lib/openzeppelin-contracts/contracts/token/ERC20/IERC20.sol/interface_IERC20.md]

### BSM (inherited from BaseEscrow)

```solidity
address public immutable BSM
```

### FEE_RECIPIENT (inherited from BaseEscrow)

```solidity
address public immutable FEE_RECIPIENT
```

### totalAssetsDeposited (inherited from BaseEscrow)

```solidity
/// @notice total user deposit amount
uint256 public totalAssetsDeposited
```

### BPS

```solidity
/// @notice Basis points representation for calculations
uint256 public constant BPS = 10_000
```

### EXTERNAL_VAULT

```solidity
/// @notice The ERC4626 compliant external vault used
IERC4626 public immutable EXTERNAL_VAULT
```

**IERC4626**: [lib/openzeppelin-contracts/contracts/interfaces/IERC4626.sol/interface_IERC4626.md]

## Errors

### CallerNotBSM (inherited from BaseEscrow)

```solidity
error CallerNotBSM();
```

### InvalidToken (inherited from BaseEscrow)

```solidity
error InvalidToken();
```

### LossCheck (inherited from BaseEscrow)

```solidity
error LossCheck();
```

### TooFewSharesReceived

```solidity
/// @notice Error for when fewer shares than expected are received from the external vault
error TooFewSharesReceived(uint256 minShares, uint256 actualShares);
```

### TooFewAssetsReceived

```solidity
/// @notice Error for when fewer assets than expected are received from the external vault
error TooFewAssetsReceived(uint256 minAssets, uint256 actualAssets);
```

## Events

### AuthorityUpdated (inherited from AuthNoOwner)

```solidity
event AuthorityUpdated(address indexed user, Authority indexed newAuthority);
```

### ProfitClaimed (inherited from IEscrow)

```solidity
event ProfitClaimed(uint256 profitAmount);
```

## Public/External Functions

### constructor(address,address,address,address,address)

- **Signature**: `constructor(address,address,address,address,address)`
- **Visibility**: public
- **Source Range**: 1580:277:39
- **Details**: [function_constructor_address_address_address_address_address.md](./function_constructor_address_address_address_address_address.md)

**Signature:**
```solidity
/// @notice Constructor for the contract
///  @param _externalVault The address of the ERC4626 compliant external vault
///  @param _assetToken The ERC20 token address used for deposits and withdrawals
///  @param _bsm The address of the BSM
///  @param _governance The governance address used for AuthNoOwner
///  @param _feeRecipient The address where collected fees are sent
constructor(address _externalVault, address _assetToken, address _bsm, address _governance, address _feeRecipient) BaseEscrow(_assetToken,_bsm,_governance,_feeRecipient);
```

### depositToExternalVault(uint256,uint256)

- **Signature**: `depositToExternalVault(uint256,uint256)`
- **Visibility**: external
- **Source Range**: 7964:375:39
- **Details**: [function_depositToExternalVault_uint256_uint256.md](./function_depositToExternalVault_uint256_uint256.md)

**Signature:**
```solidity
/// @notice Deposits assets into the external vault
///  @dev Can only be called by authorized users
///  @dev Can cause losses if the admin is not careful
///  @dev Assumes: Slippage check is safe
///  @param assetsToDeposit The amount of assets to deposit
///  @param minShares The minimum acceptable shares to receive for the deposited assets
function depositToExternalVault(uint256 assetsToDeposit, uint256 minShares) external requiresAuth();
```

### redeemFromExternalVault(uint256,uint256)

- **Signature**: `redeemFromExternalVault(uint256,uint256)`
- **Visibility**: external
- **Source Range**: 8603:303:39
- **Details**: [function_redeemFromExternalVault_uint256_uint256.md](./function_redeemFromExternalVault_uint256_uint256.md)

**Signature:**
```solidity
/// @notice Redeems shares from the external vault
///  @dev Can only be called by authorized users
///  @param sharesToRedeem The number of shares to redeem
///  @param minAssets The minimum acceptable assets to receive for the redeemed shares
function redeemFromExternalVault(uint256 sharesToRedeem, uint256 minAssets) external requiresAuth();
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

### constructor(address,address,address,address) (inherited from BaseEscrow)

- **Signature**: `constructor(address,address,address,address)`
- **Visibility**: public
- **Source Range**: 1239:369:22
- **Details**: [function_constructor_address_address_address_address.md](./function_constructor_address_address_address_address.md)

**Signature:**
```solidity
/// @notice Contract constructor
///  @param _assetToken The ERC20 token address used for deposits and withdrawals
///  @param _bsm The address of the eBTC Stability Module (BSM)
///  @param _governance The governance address used for AuthNoOwner
///  @param _feeRecipient The address where collected fees are sent
constructor(address _assetToken, address _bsm, address _governance, address _feeRecipient);
```

### totalBalance() (inherited from BaseEscrow)

- **Signature**: `totalBalance()`
- **Visibility**: external
- **Source Range**: 3897:95:22
- **Details**: [function_totalBalance.md](./function_totalBalance.md)

**Signature:**
```solidity
/// @notice Returns the total balance of assets in the escrow
function totalBalance() external view returns (uint256);
```

### onDeposit(uint256) (inherited from BaseEscrow)

- **Signature**: `onDeposit(uint256)`
- **Visibility**: external
- **Source Range**: 4158:99:22
- **Details**: [function_onDeposit_uint256.md](./function_onDeposit_uint256.md)

**Signature:**
```solidity
/// @notice Deposits assets into the escrow
///  @dev Can only be called by the bsm contract
///  @param _assetAmount The amount of assets to deposit
function onDeposit(uint256 _assetAmount) external onlyBSM();
```

### onWithdraw(uint256) (inherited from BaseEscrow)

- **Signature**: `onWithdraw(uint256)`
- **Visibility**: external
- **Source Range**: 4472:126:22
- **Details**: [function_onWithdraw_uint256.md](./function_onWithdraw_uint256.md)

**Signature:**
```solidity
/// @notice Withdraws assets from the escrow
///  @dev Can only be called by the bsm contract
///  @param _assetAmount The amount of assets to withdraw
///  @return The amount of assets withdrawn
function onWithdraw(uint256 _assetAmount) external onlyBSM() returns (uint256);
```

### previewWithdraw(uint256) (inherited from BaseEscrow)

- **Signature**: `previewWithdraw(uint256)`
- **Visibility**: external
- **Source Range**: 4604:225:22
- **Details**: [function_previewWithdraw_uint256.md](./function_previewWithdraw_uint256.md)

**Signature:**
```solidity
function previewWithdraw(uint256 _assetAmount) external view returns (uint256);
```

### onMigrateSource(address) (inherited from BaseEscrow)

- **Signature**: `onMigrateSource(address)`
- **Visibility**: external
- **Source Range**: 5028:519:22
- **Details**: [function_onMigrateSource_address.md](./function_onMigrateSource_address.md)

**Signature:**
```solidity
/// @notice Called on the source escrow during a migration by the BSM to transfer liquidity
///  @dev Can only be called by the bsm contract
///  @param _newEscrow new escrow address
function onMigrateSource(address _newEscrow) external onlyBSM();
```

### onMigrateTarget(uint256) (inherited from BaseEscrow)

- **Signature**: `onMigrateTarget(uint256)`
- **Visibility**: external
- **Source Range**: 5710:106:22
- **Details**: [function_onMigrateTarget_uint256.md](./function_onMigrateTarget_uint256.md)

**Signature:**
```solidity
/// @notice Called on the target escrow during a migration by the BSM to set the user deposit amount
///  @dev Can only be called by the bsm contract
function onMigrateTarget(uint256 _amount) external onlyBSM();
```

### feeProfit() (inherited from BaseEscrow)

- **Signature**: `feeProfit()`
- **Visibility**: public
- **Source Range**: 5939:253:22
- **Details**: [function_feeProfit.md](./function_feeProfit.md)

**Signature:**
```solidity
/// @notice Calculates the profit generated from asset management
///  @return The amount of profit generated
function feeProfit() public view returns (uint256);
```

### claimProfit() (inherited from BaseEscrow)

- **Signature**: `claimProfit()`
- **Visibility**: external
- **Source Range**: 6312:76:22
- **Details**: [function_claimProfit.md](./function_claimProfit.md)

**Signature:**
```solidity
/// @notice Claim profit (fees + external lending profit)
///  @dev can only be called by authorized users
function claimProfit() external requiresAuth();
```

### claimTokens(address,uint256) (inherited from BaseEscrow)

- **Signature**: `claimTokens(address,uint256)`
- **Visibility**: external
- **Source Range**: 6718:118:22
- **Details**: [function_claimTokens_address_uint256.md](./function_claimTokens_address_uint256.md)

**Signature:**
```solidity
/// @notice Claim reward tokens and/or other tokens sent to this contract
function claimTokens(address token, uint256 amount) external requiresAuth();
```
