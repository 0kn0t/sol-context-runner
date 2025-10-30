# Contract: BaseEscrow

## Metadata

- **Name**: BaseEscrow
- **Type**: Contract
- **Path**: src/BaseEscrow.sol
- **Documentation**: @title BaseEscrow
   @notice Handles assets custody on deposits and withdrawals.

## Implements Interfaces

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

### ASSET_TOKEN

```solidity
IERC20 public immutable ASSET_TOKEN
```

**IERC20**: [lib/openzeppelin-contracts/contracts/token/ERC20/IERC20.sol/interface_IERC20.md]

### BSM

```solidity
address public immutable BSM
```

### FEE_RECIPIENT

```solidity
address public immutable FEE_RECIPIENT
```

### totalAssetsDeposited

```solidity
/// @notice total user deposit amount
uint256 public totalAssetsDeposited
```

## Errors

### CallerNotBSM

```solidity
error CallerNotBSM();
```

### InvalidToken

```solidity
error InvalidToken();
```

### LossCheck

```solidity
error LossCheck();
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

### constructor(address,address,address,address)

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

### totalBalance()

- **Signature**: `totalBalance()`
- **Visibility**: external
- **Source Range**: 3897:95:22
- **Details**: [function_totalBalance.md](./function_totalBalance.md)

**Signature:**
```solidity
/// @notice Returns the total balance of assets in the escrow
function totalBalance() external view returns (uint256);
```

### onDeposit(uint256)

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

### onWithdraw(uint256)

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

### previewWithdraw(uint256)

- **Signature**: `previewWithdraw(uint256)`
- **Visibility**: external
- **Source Range**: 4604:225:22
- **Details**: [function_previewWithdraw_uint256.md](./function_previewWithdraw_uint256.md)

**Signature:**
```solidity
function previewWithdraw(uint256 _assetAmount) external view returns (uint256);
```

### onMigrateSource(address)

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

### onMigrateTarget(uint256)

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

### feeProfit()

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

### claimProfit()

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

### claimTokens(address,uint256)

- **Signature**: `claimTokens(address,uint256)`
- **Visibility**: external
- **Source Range**: 6718:118:22
- **Details**: [function_claimTokens_address_uint256.md](./function_claimTokens_address_uint256.md)

**Signature:**
```solidity
/// @notice Claim reward tokens and/or other tokens sent to this contract
function claimTokens(address token, uint256 amount) external requiresAuth();
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
