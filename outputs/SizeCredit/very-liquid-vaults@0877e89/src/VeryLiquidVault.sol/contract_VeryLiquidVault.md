# Contract: VeryLiquidVault

## Metadata

- **Name**: VeryLiquidVault
- **Type**: Contract
- **Path**: src/VeryLiquidVault.sol
- **Documentation**: @title VeryLiquidVault
   @custom:security-contact security@size.credit
   @author Size (https://size.credit/)
   @notice Very Liquid Vault that distributes assets across multiple strategies
   @dev Extends PerformanceVault to manage multiple strategy vaults for asset allocation. By default, the performance fee is 0.

## Implements Interfaces

- **IERC1822Proxiable** [lib/openzeppelin-contracts/contracts/interfaces/draft-IERC1822.sol/interface_IERC1822Proxiable.md]
- **IERC5267** [lib/openzeppelin-contracts/contracts/interfaces/IERC5267.sol/interface_IERC5267.md]
- **IERC20Permit** [lib/openzeppelin-contracts/contracts/token/ERC20/extensions/IERC20Permit.sol/interface_IERC20Permit.md]
- **IVault** [src/IVault.sol/interface_IVault.md]
- **IERC4626** [lib/openzeppelin-contracts/contracts/interfaces/IERC4626.sol/interface_IERC4626.md]
- **IERC20Errors** [lib/openzeppelin-contracts/contracts/interfaces/draft-IERC6093.sol/interface_IERC20Errors.md]
- **IERC20Metadata** [lib/openzeppelin-contracts/contracts/token/ERC20/extensions/IERC20Metadata.sol/interface_IERC20Metadata.md]
- **IERC20** [lib/openzeppelin-contracts/contracts/token/ERC20/IERC20.sol/interface_IERC20.md]

## State Variables

### INITIALIZABLE_STORAGE (inherited from Initializable)

```solidity
bytes32 private constant INITIALIZABLE_STORAGE = 0xf0c57e16840df040f15088dc2f81fe391c3923bec73e23a9662efc9c229c6a00
```

### ERC20StorageLocation (inherited from ERC20Upgradeable)

```solidity
bytes32 private constant ERC20StorageLocation = 0x52c63247e1f47db19d5ce0460030c497f067ca4cebf71ba98eeadabe20bace00
```

### ERC4626StorageLocation (inherited from ERC4626Upgradeable)

```solidity
bytes32 private constant ERC4626StorageLocation = 0x0773e532dfede91f04b12a73d3d2acd361424f41f76b4fb79f090161e36b4e00
```

### TYPE_HASH (inherited from EIP712Upgradeable)

```solidity
bytes32 private constant TYPE_HASH = keccak256("EIP712Domain(string name,string version,uint256 chainId,address verifyingContract)")
```

### EIP712StorageLocation (inherited from EIP712Upgradeable)

```solidity
bytes32 private constant EIP712StorageLocation = 0xa16a46d94261c7517cc8ff89f61c0ce93598e3c849801011dee649a6a557d100
```

### NoncesStorageLocation (inherited from NoncesUpgradeable)

```solidity
bytes32 private constant NoncesStorageLocation = 0x5ab42ced628888259c08ac98db1eb0cf702fc1501344311d8b100cd1bfe4bb00
```

### PERMIT_TYPEHASH (inherited from ERC20PermitUpgradeable)

```solidity
bytes32 private constant PERMIT_TYPEHASH = keccak256("Permit(address owner,address spender,uint256 value,uint256 nonce,uint256 deadline)")
```

### NOT_ENTERED (inherited from ReentrancyGuardUpgradeable)

```solidity
uint256 private constant NOT_ENTERED = 1
```

### ENTERED (inherited from ReentrancyGuardUpgradeable)

```solidity
uint256 private constant ENTERED = 2
```

### ReentrancyGuardStorageLocation (inherited from ReentrancyGuardUpgradeable)

```solidity
bytes32 private constant ReentrancyGuardStorageLocation = 0x9b779b17422d0df92223018b32b4d1fa46e071723d6817e2486d003becc55f00
```

### PausableStorageLocation (inherited from PausableUpgradeable)

```solidity
bytes32 private constant PausableStorageLocation = 0xcd5ed15c6e187e77e9aee88184c21f4f2182ab5827cb3b7e07fbedcd63f03300
```

### __self (inherited from UUPSUpgradeable)

```solidity
/// @custom:oz-upgrades-unsafe-allow state-variable-immutable
address private immutable __self = address(this)
```

### UPGRADE_INTERFACE_VERSION (inherited from UUPSUpgradeable)

```solidity
///  @dev The version of the upgrade interface of the contract. If this getter is missing, both `upgradeTo(address)`
///  and `upgradeToAndCall(address,bytes)` are present, and `upgradeTo` must be used if no function should be called,
///  while `upgradeToAndCall` will invoke the `receive` function if the second argument is the empty byte string.
///  If the getter returns `"5.0.0"`, only `upgradeToAndCall(address,bytes)` is present, and the second argument must
///  be the empty byte string if no function should be called, making it impossible to invoke the `receive` function
///  during an upgrade.
string public constant UPGRADE_INTERFACE_VERSION = "5.0.0"
```

### PERCENT (inherited from BaseVault)

```solidity
/// @dev Constant representing 100%
uint256 internal constant PERCENT = 1e18
```

### BaseVaultStorageLocation (inherited from BaseVault)

```solidity
bytes32 private constant BaseVaultStorageLocation = 0x83cbba01667a5ddf3820f0a2c4220dbc355a1a788c7094daad71b73a418b0d00
```

### MAXIMUM_PERFORMANCE_FEE_PERCENT (inherited from PerformanceVault)

```solidity
/// @dev Constant representing the maximum performance fee in PERCENT
uint256 private constant MAXIMUM_PERFORMANCE_FEE_PERCENT = 0.5e18
```

### PerformanceVaultStorageLocation (inherited from PerformanceVault)

```solidity
bytes32 private constant PerformanceVaultStorageLocation = 0x8133214f360747da779e5436efd7332b025d67830fc92a23fa9b9e5e4b87fd00
```

### MAX_STRATEGIES

```solidity
/// @dev The maximum number of strategies that can be added to the vault
uint256 private constant MAX_STRATEGIES = 10
```

### DEFAULT_MAX_SLIPPAGE_PERCENT

```solidity
/// @dev The default maximum slippage percent for rebalancing in PERCENT
uint256 private constant DEFAULT_MAX_SLIPPAGE_PERCENT = 0.01e18
```

### VeryLiquidVaultStorageLocation

```solidity
bytes32 private constant VeryLiquidVaultStorageLocation = 0x851713d8b7886cdb5682ccb4d2dba1bf8cae30c699ce588016da31dab5d7f100
```

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

### ERC20Storage (inherited from ERC20Upgradeable)

```solidity
/// @custom:storage-location erc7201:openzeppelin.storage.ERC20
struct ERC20Storage {
    mapping(address => uint256) _balances;
    mapping(address => mapping(address => uint256)) _allowances;
    uint256 _totalSupply;
    string _name;
    string _symbol;
}
```

### ERC4626Storage (inherited from ERC4626Upgradeable)

```solidity
/// @custom:storage-location erc7201:openzeppelin.storage.ERC4626
struct ERC4626Storage {
    IERC20 _asset;
    uint8 _underlyingDecimals;
}
```

### EIP712Storage (inherited from EIP712Upgradeable)

```solidity
/// @custom:storage-location erc7201:openzeppelin.storage.EIP712
struct EIP712Storage {
    bytes32 _hashedName;
    bytes32 _hashedVersion;
    string _name;
    string _version;
}
```

### NoncesStorage (inherited from NoncesUpgradeable)

```solidity
/// @custom:storage-location erc7201:openzeppelin.storage.Nonces
struct NoncesStorage {
    mapping(address => uint256) _nonces;
}
```

### ReentrancyGuardStorage (inherited from ReentrancyGuardUpgradeable)

```solidity
/// @custom:storage-location erc7201:openzeppelin.storage.ReentrancyGuard
struct ReentrancyGuardStorage {
    uint256 _status;
}
```

### PausableStorage (inherited from PausableUpgradeable)

```solidity
/// @custom:storage-location erc7201:openzeppelin.storage.Pausable
struct PausableStorage {
    bool _paused;
}
```

### BaseVaultStorage (inherited from BaseVault)

```solidity
/// @custom:storage-location erc7201:vlv.storage.BaseVault
struct BaseVaultStorage {
    Auth _auth;
    uint256 _totalAssetsCap;
}
```

### PerformanceVaultStorage (inherited from PerformanceVault)

```solidity
/// @custom:storage-location erc7201:vlv.storage.PerformanceVault
struct PerformanceVaultStorage {
    uint256 _highWaterMark;
    uint256 _performanceFeePercent;
    address _feeRecipient;
}
```

### VeryLiquidVaultStorage

```solidity
/// @custom:storage-location erc7201:vlv.storage.VeryLiquidVault
struct VeryLiquidVaultStorage {
    IVault[] _strategies;
    uint256 _rebalanceMaxSlippagePercent;
}
```

## Errors

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

### ERC20InsufficientBalance (inherited from IERC20Errors)

```solidity
///  @dev Indicates an error related to the current `balance` of a `sender`. Used in transfers.
///  @param sender Address whose tokens are being transferred.
///  @param balance Current balance for the interacting account.
///  @param needed Minimum amount required to perform a transfer.
error ERC20InsufficientBalance(address sender, uint256 balance, uint256 needed);
```

### ERC20InvalidSender (inherited from IERC20Errors)

```solidity
///  @dev Indicates a failure with the token `sender`. Used in transfers.
///  @param sender Address whose tokens are being transferred.
error ERC20InvalidSender(address sender);
```

### ERC20InvalidReceiver (inherited from IERC20Errors)

```solidity
///  @dev Indicates a failure with the token `receiver`. Used in transfers.
///  @param receiver Address to which tokens are being transferred.
error ERC20InvalidReceiver(address receiver);
```

### ERC20InsufficientAllowance (inherited from IERC20Errors)

```solidity
///  @dev Indicates a failure with the `spender`’s `allowance`. Used in transfers.
///  @param spender Address that may be allowed to operate on tokens without being their owner.
///  @param allowance Amount of tokens a `spender` is allowed to operate with.
///  @param needed Minimum amount required to perform a transfer.
error ERC20InsufficientAllowance(address spender, uint256 allowance, uint256 needed);
```

### ERC20InvalidApprover (inherited from IERC20Errors)

```solidity
///  @dev Indicates a failure with the `approver` of a token to be approved. Used in approvals.
///  @param approver Address initiating an approval operation.
error ERC20InvalidApprover(address approver);
```

### ERC20InvalidSpender (inherited from IERC20Errors)

```solidity
///  @dev Indicates a failure with the `spender` to be approved. Used in approvals.
///  @param spender Address that may be allowed to operate on tokens without being their owner.
error ERC20InvalidSpender(address spender);
```

### ERC4626ExceededMaxDeposit (inherited from ERC4626Upgradeable)

```solidity
///  @dev Attempted to deposit more assets than the max amount for `receiver`.
error ERC4626ExceededMaxDeposit(address receiver, uint256 assets, uint256 max);
```

### ERC4626ExceededMaxMint (inherited from ERC4626Upgradeable)

```solidity
///  @dev Attempted to mint more shares than the max amount for `receiver`.
error ERC4626ExceededMaxMint(address receiver, uint256 shares, uint256 max);
```

### ERC4626ExceededMaxWithdraw (inherited from ERC4626Upgradeable)

```solidity
///  @dev Attempted to withdraw more assets than the max amount for `receiver`.
error ERC4626ExceededMaxWithdraw(address owner, uint256 assets, uint256 max);
```

### ERC4626ExceededMaxRedeem (inherited from ERC4626Upgradeable)

```solidity
///  @dev Attempted to redeem more shares than the max amount for `receiver`.
error ERC4626ExceededMaxRedeem(address owner, uint256 shares, uint256 max);
```

### InvalidAccountNonce (inherited from NoncesUpgradeable)

```solidity
///  @dev The nonce used for an `account` is not the expected current nonce.
error InvalidAccountNonce(address account, uint256 currentNonce);
```

### ERC2612ExpiredSignature (inherited from ERC20PermitUpgradeable)

```solidity
///  @dev Permit deadline has expired.
error ERC2612ExpiredSignature(uint256 deadline);
```

### ERC2612InvalidSigner (inherited from ERC20PermitUpgradeable)

```solidity
///  @dev Mismatched signature.
error ERC2612InvalidSigner(address signer, address owner);
```

### ReentrancyGuardReentrantCall (inherited from ReentrancyGuardUpgradeable)

```solidity
///  @dev Unauthorized reentrant call.
error ReentrancyGuardReentrantCall();
```

### EnforcedPause (inherited from PausableUpgradeable)

```solidity
///  @dev The operation failed because the contract is paused.
error EnforcedPause();
```

### ExpectedPause (inherited from PausableUpgradeable)

```solidity
///  @dev The operation failed because the contract is not paused.
error ExpectedPause();
```

### UUPSUnauthorizedCallContext (inherited from UUPSUpgradeable)

```solidity
///  @dev The call is from an unauthorized context.
error UUPSUnauthorizedCallContext();
```

### UUPSUnsupportedProxiableUUID (inherited from UUPSUpgradeable)

```solidity
///  @dev The storage `slot` is unsupported as a UUID.
error UUPSUnsupportedProxiableUUID(bytes32 slot);
```

### NullAddress (inherited from BaseVault)

```solidity
error NullAddress();
```

### NullAmount (inherited from BaseVault)

```solidity
error NullAmount();
```

### InvalidAsset (inherited from BaseVault)

```solidity
error InvalidAsset(address asset);
```

### PerformanceFeePercentTooHigh (inherited from PerformanceVault)

```solidity
error PerformanceFeePercentTooHigh(uint256 performanceFeePercent, uint256 maximumPerformanceFeePercent);
```

### InvalidStrategy

```solidity
error InvalidStrategy(address strategy);
```

### CannotDepositToStrategies

```solidity
error CannotDepositToStrategies(uint256 assets, uint256 shares, uint256 remainingAssets);
```

### CannotWithdrawFromStrategies

```solidity
error CannotWithdrawFromStrategies(uint256 assets, uint256 shares, uint256 missingAssets);
```

### TransferredAmountLessThanMin

```solidity
error TransferredAmountLessThanMin(uint256 assetsBefore, uint256 assetsAfter, uint256 slippage, uint256 amount, uint256 maxSlippagePercent);
```

### MaxStrategiesExceeded

```solidity
error MaxStrategiesExceeded(uint256 strategiesCount, uint256 maxStrategies);
```

### ArrayLengthMismatch

```solidity
error ArrayLengthMismatch(uint256 expectedLength, uint256 actualLength);
```

### InvalidMaxSlippagePercent

```solidity
error InvalidMaxSlippagePercent(uint256 maxSlippagePercent);
```

## Events

### Initialized (inherited from Initializable)

```solidity
///  @dev Triggered when the contract has been initialized or reinitialized.
event Initialized(uint64 version);
```

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

### Deposit (inherited from IERC4626)

```solidity
event Deposit(address indexed sender, address indexed owner, uint256 assets, uint256 shares);
```

### Withdraw (inherited from IERC4626)

```solidity
event Withdraw(address indexed sender, address indexed receiver, address indexed owner, uint256 assets, uint256 shares);
```

### EIP712DomainChanged (inherited from IERC5267)

```solidity
///  @dev MAY be emitted to signal that the domain could have changed.
event EIP712DomainChanged();
```

### Paused (inherited from PausableUpgradeable)

```solidity
///  @dev Emitted when the pause is triggered by `account`.
event Paused(address account);
```

### Unpaused (inherited from PausableUpgradeable)

```solidity
///  @dev Emitted when the pause is lifted by `account`.
event Unpaused(address account);
```

### AuthSet (inherited from BaseVault)

```solidity
event AuthSet(address indexed auth);
```

### TotalAssetsCapSet (inherited from BaseVault)

```solidity
event TotalAssetsCapSet(uint256 indexed totalAssetsCapBefore, uint256 indexed totalAssetsCapAfter);
```

### VaultStatus (inherited from BaseVault)

```solidity
event VaultStatus(uint256 totalShares, uint256 totalAssets);
```

### PerformanceFeePercentSet (inherited from PerformanceVault)

```solidity
event PerformanceFeePercentSet(uint256 indexed performanceFeePercentBefore, uint256 indexed performanceFeePercentAfter);
```

### FeeRecipientSet (inherited from PerformanceVault)

```solidity
event FeeRecipientSet(address indexed feeRecipientBefore, address indexed feeRecipientAfter);
```

### HighWaterMarkUpdated (inherited from PerformanceVault)

```solidity
event HighWaterMarkUpdated(uint256 highWaterMarkBefore, uint256 highWaterMarkAfter);
```

### PerformanceFeeMinted (inherited from PerformanceVault)

```solidity
event PerformanceFeeMinted(address indexed to, uint256 shares, uint256 assets);
```

### StrategyAdded

```solidity
event StrategyAdded(address indexed strategy);
```

### StrategyRemoved

```solidity
event StrategyRemoved(address indexed strategy);
```

### StrategyReordered

```solidity
event StrategyReordered(address indexed strategyOld, address indexed strategyNew, uint256 indexed index);
```

### Rebalanced

```solidity
event Rebalanced(address indexed strategyFrom, address indexed strategyTo, uint256 rebalancedAmount, uint256 maxSlippagePercent);
```

### RebalanceMaxSlippagePercentSet

```solidity
event RebalanceMaxSlippagePercentSet(uint256 oldRebalanceMaxSlippagePercent, uint256 newRebalanceMaxSlippagePercent);
```

### DepositFailed

```solidity
event DepositFailed(address indexed strategy, uint256 amount);
```

### WithdrawFailed

```solidity
event WithdrawFailed(address indexed strategy, uint256 amount);
```

## Public/External Functions

### constructor()

- **Signature**: `constructor()`
- **Visibility**: public
- **Source Range**: 3428:53:145
- **Details**: [function_constructor.md](./function_constructor.md)

**Signature:**
```solidity
/// @custom:oz-upgrades-unsafe-allow constructor
constructor();
```

### initialize(contract Auth,contract IERC20,string,string,address,uint256,contract IVault[])

- **Signature**: `initialize(contract Auth,contract IERC20,string,string,address,uint256,contract IVault[])`
- **Visibility**: public
- **Source Range**: 4041:658:145
- **Details**: [function_initialize_contract_Auth_contract_IERC20_string_string_address_uint256_contract_IVault[].md](./function_initialize_contract_Auth_contract_IERC20_string_string_address_uint256_contract_IVault[].md)

**Signature:**
```solidity
/// @notice Initializes the VeryLiquidVault with strategies
///  @param auth_ The address of the Auth contract
///  @param asset_ The address of the asset
///  @param name_ The name of the vault
///  @param symbol_ The symbol of the vault
///  @param fundingAccount The address of the funding account for the first deposit, which will be treated as dead shares
///  @param firstDepositAmount The amount of the first deposit, which will be treated as dead shares
///  @param strategies_ The initial strategies to add to the vault
function initialize(Auth auth_, IERC20 asset_, string memory name_, string memory symbol_, address fundingAccount, uint256 firstDepositAmount, IVault[] memory strategies_) virtual public initializer();
```

### maxDeposit(address)

- **Signature**: `maxDeposit(address)`
- **Visibility**: public
- **Source Range**: 4944:175:145
- **Details**: [function_maxDeposit_address.md](./function_maxDeposit_address.md)

**Signature:**
```solidity
/// @inheritdoc ERC4626Upgradeable
///  @dev The maximum amount that can be deposited is the minimum between this receiver specific limit and the maximum asset amount that can be deposited to all strategies
function maxDeposit(address receiver) override(BaseVault) public view returns (uint256);
```

### maxMint(address)

- **Signature**: `maxMint(address)`
- **Visibility**: public
- **Source Range**: 5354:459:145
- **Details**: [function_maxMint_address.md](./function_maxMint_address.md)

**Signature:**
```solidity
/// @inheritdoc ERC4626Upgradeable
///  @dev The maximum amount that can be minted is the minimum between this receiver specific limit and the maximum asset amount that can be minted to all strategies, converted to shares
function maxMint(address receiver) override(BaseVault) public view returns (uint256);
```

### maxWithdraw(address)

- **Signature**: `maxWithdraw(address)`
- **Visibility**: public
- **Source Range**: 6032:174:145
- **Details**: [function_maxWithdraw_address.md](./function_maxWithdraw_address.md)

**Signature:**
```solidity
/// @inheritdoc ERC4626Upgradeable
///  @dev The maximum amount that can be withdrawn is the minimum between this owner specific limit and the maximum asset amount that can be withdrawn from all strategies
function maxWithdraw(address owner) override(BaseVault) public view returns (uint256);
```

### maxRedeem(address)

- **Signature**: `maxRedeem(address)`
- **Visibility**: public
- **Source Range**: 6444:451:145
- **Details**: [function_maxRedeem_address.md](./function_maxRedeem_address.md)

**Signature:**
```solidity
/// @inheritdoc ERC4626Upgradeable
///  @dev The maximum amount that can be redeemed is the minimum between this owner specific limit and the maximum asset amount that can be redeemed from all strategies, converted to shares
function maxRedeem(address owner) override(BaseVault) public view returns (uint256);
```

### totalAssets()

- **Signature**: `totalAssets()`
- **Visibility**: public
- **Source Range**: 7057:583:145
- **Details**: [function_totalAssets.md](./function_totalAssets.md)

**Signature:**
```solidity
/// @inheritdoc ERC4626Upgradeable
///  @dev The total assets is the sum of the assets in all strategies
function totalAssets() virtual override(ERC4626Upgradeable, IERC4626) public view returns (uint256 total);
```

### setPerformanceFeePercent(uint256)

- **Signature**: `setPerformanceFeePercent(uint256)`
- **Visibility**: external
- **Source Range**: 10872:211:145
- **Details**: [function_setPerformanceFeePercent_uint256.md](./function_setPerformanceFeePercent_uint256.md)

**Signature:**
```solidity
/// @notice Sets the performance fee percent
///  @param performanceFeePercent_ The new performance fee percent
///  @dev Only callable by addresses with DEFAULT_ADMIN_ROLE
function setPerformanceFeePercent(uint256 performanceFeePercent_) external nonReentrant() onlyAuth(DEFAULT_ADMIN_ROLE);
```

### setFeeRecipient(address)

- **Signature**: `setFeeRecipient(address)`
- **Visibility**: external
- **Source Range**: 11243:147:145
- **Details**: [function_setFeeRecipient_address.md](./function_setFeeRecipient_address.md)

**Signature:**
```solidity
/// @notice Sets the fee recipient
///  @param feeRecipient_ The new fee recipient
///  @dev Only callable by addresses with DEFAULT_ADMIN_ROLE
function setFeeRecipient(address feeRecipient_) external nonReentrant() onlyAuth(DEFAULT_ADMIN_ROLE);
```

### setRebalanceMaxSlippagePercent(uint256)

- **Signature**: `setRebalanceMaxSlippagePercent(uint256)`
- **Visibility**: external
- **Source Range**: 11630:235:145
- **Details**: [function_setRebalanceMaxSlippagePercent_uint256.md](./function_setRebalanceMaxSlippagePercent_uint256.md)

**Signature:**
```solidity
/// @notice Sets the rebalance max slippage percent
///  @param rebalanceMaxSlippagePercent_ The new rebalance max slippage percent
///  @dev Only callable by addresses with VAULT_MANAGER_ROLE
function setRebalanceMaxSlippagePercent(uint256 rebalanceMaxSlippagePercent_) external nonReentrant() onlyAuth(VAULT_MANAGER_ROLE);
```

### addStrategy(contract IVault)

- **Signature**: `addStrategy(contract IVault)`
- **Visibility**: external
- **Source Range**: 12033:172:145
- **Details**: [function_addStrategy_contract_IVault.md](./function_addStrategy_contract_IVault.md)

**Signature:**
```solidity
/// @notice Adds a new strategy to the vault
///  @param strategy_ The new strategy to add
///  @dev Only callable by addresses with VAULT_MANAGER_ROLE
function addStrategy(IVault strategy_) external nonReentrant() emitVaultStatus() onlyAuth(VAULT_MANAGER_ROLE);
```

### removeStrategy(contract IVault,contract IVault,uint256,uint256)

- **Signature**: `removeStrategy(contract IVault,contract IVault,uint256,uint256)`
- **Visibility**: external
- **Source Range**: 13225:1005:145
- **Details**: [function_removeStrategy_contract_IVault_contract_IVault_uint256_uint256.md](./function_removeStrategy_contract_IVault_contract_IVault_uint256_uint256.md)

**Signature:**
```solidity
/// @notice Removes a strategy from the vault and transfers all assets, if any, to another strategy
///  @param strategyToRemove The strategy to remove
///  @param strategyToReceiveAssets The strategy to receive the assets
///  @param amount The amount of assets to transfer
///  @param maxSlippagePercent The maximum slippage percent allowed for the rebalance
///  @dev Only callable by addresses with GUARDIAN_ROLE
///  @dev Using `amount` = 0 will forfeit all assets from `strategyToRemove`
///  @dev Using `amount` = type(uint256).max will attempt to transfer the entire balance from `strategyToRemove`
///  @dev If `convertToAssets(balanceOf)` > `maxWithdraw`, e.g. due to pause/withdraw limits, the _rebalance step will revert, so an appropriate `amount` should be used
///  @dev Reverts if totalAssets() == 0 at the end of the operation, which can happen if the call is performed with 100% slippage
function removeStrategy(IVault strategyToRemove, IVault strategyToReceiveAssets, uint256 amount, uint256 maxSlippagePercent) external nonReentrant() emitVaultStatus() onlyAuth(GUARDIAN_ROLE);
```

### reorderStrategies(contract IVault[])

- **Signature**: `reorderStrategies(contract IVault[])`
- **Visibility**: external
- **Source Range**: 14588:1054:145
- **Details**: [function_reorderStrategies_contract_IVault[].md](./function_reorderStrategies_contract_IVault[].md)

**Signature:**
```solidity
/// @notice Reorders the strategies
///  @param newStrategiesOrder The new strategies order
///  @dev Only callable by addresses with STRATEGIST_ROLE
///  @dev Verifies that the new strategies order is valid and that there are no duplicates
///  @dev Clears current strategies and adds them in the new order
function reorderStrategies(IVault[] calldata newStrategiesOrder) external nonReentrant() notPaused() onlyAuth(STRATEGIST_ROLE);
```

### rebalance(contract IVault,contract IVault,uint256,uint256)

- **Signature**: `rebalance(contract IVault,contract IVault,uint256,uint256)`
- **Visibility**: external
- **Source Range**: 16239:772:145
- **Details**: [function_rebalance_contract_IVault_contract_IVault_uint256_uint256.md](./function_rebalance_contract_IVault_contract_IVault_uint256_uint256.md)

**Signature:**
```solidity
/// @notice Rebalances assets between two strategies
///  @param strategyFrom The strategy to transfer assets from
///  @param strategyTo The strategy to transfer assets to
///  @param amount The amount of assets to transfer
///  @param maxSlippagePercent The maximum slippage percent allowed for the rebalance
///  @dev Only callable by addresses with STRATEGIST_ROLE
///  @dev Transfers assets from one strategy to another
///  @dev We have maxSlippagePercent <= PERCENT since rebalanceMaxSlippagePercent has already been checked in setRebalanceMaxSlippagePercent
function rebalance(IVault strategyFrom, IVault strategyTo, uint256 amount, uint256 maxSlippagePercent) external nonReentrant() notPaused() emitVaultStatus() onlyAuth(STRATEGIST_ROLE);
```

### strategies()

- **Signature**: `strategies()`
- **Visibility**: public
- **Source Range**: 24032:114:145
- **Details**: [function_strategies.md](./function_strategies.md)

**Signature:**
```solidity
/// @notice Returns the strategies in the vault
///  @return The strategies in the vault
function strategies() public view nonReentrantView() returns (IVault[] memory);
```

### strategies(uint256)

- **Signature**: `strategies(uint256)`
- **Visibility**: public
- **Source Range**: 24504:152:145
- **Details**: [function_strategies_uint256.md](./function_strategies_uint256.md)

**Signature:**
```solidity
/// @notice Returns the strategy at the given index
///  @param index The index of the strategy
///  @return The strategy at the given index
function strategies(uint256 index) public view nonReentrantView() returns (IVault);
```

### strategiesCount()

- **Signature**: `strategiesCount()`
- **Visibility**: public
- **Source Range**: 24778:117:145
- **Details**: [function_strategiesCount.md](./function_strategiesCount.md)

**Signature:**
```solidity
/// @notice Returns the number of strategies in the vault
///  @return The number of strategies in the vault
function strategiesCount() public view nonReentrantView() returns (uint256);
```

### rebalanceMaxSlippagePercent()

- **Signature**: `rebalanceMaxSlippagePercent()`
- **Visibility**: public
- **Source Range**: 25011:140:145
- **Details**: [function_rebalanceMaxSlippagePercent.md](./function_rebalanceMaxSlippagePercent.md)

**Signature:**
```solidity
/// @notice Returns the rebalance max slippage percent
///  @return The rebalance max slippage percent
function rebalanceMaxSlippagePercent() public view nonReentrantView() returns (uint256);
```

### isStrategy(contract IVault)

- **Signature**: `isStrategy(contract IVault)`
- **Visibility**: public
- **Source Range**: 25551:126:145
- **Details**: [function_isStrategy_contract_IVault.md](./function_isStrategy_contract_IVault.md)

**Signature:**
```solidity
/// @notice Returns true if the strategy is in the vault
///  @param strategy The strategy to check
///  @return True if the strategy is in the vault
function isStrategy(IVault strategy) public view nonReentrantView() returns (bool);
```

### name() (inherited from ERC20Upgradeable)

- **Signature**: `name()`
- **Visibility**: public
- **Source Range**: 2697:144:58
- **Details**: [function_name.md](./function_name.md)

**Signature:**
```solidity
///  @dev Returns the name of the token.
function name() virtual public view returns (string memory);
```

### symbol() (inherited from ERC20Upgradeable)

- **Signature**: `symbol()`
- **Visibility**: public
- **Source Range**: 2954:148:58
- **Details**: [function_symbol.md](./function_symbol.md)

**Signature:**
```solidity
///  @dev Returns the symbol of the token, usually a shorter version of the
///  name.
function symbol() virtual public view returns (string memory);
```

### decimals() (inherited from ERC20Upgradeable)

- **Signature**: `decimals()`
- **Visibility**: public
- **Source Range**: 3735:82:58
- **Details**: [function_decimals.md](./function_decimals.md)

**Signature:**
```solidity
///  @dev Returns the number of decimals used to get its user representation.
///  For example, if `decimals` equals `2`, a balance of `505` tokens should
///  be displayed to a user as `5.05` (`505 / 10 ** 2`).
///  Tokens usually opt for a value of 18, imitating the relationship between
///  Ether and Wei. This is the default value returned by this function, unless
///  it's overridden.
///  NOTE: This information is only used for _display_ purposes: it in
///  no way affects any of the arithmetic of the contract, including
///  {IERC20-balanceOf} and {IERC20-transfer}.
function decimals() virtual public view returns (uint8);
```

### totalSupply() (inherited from ERC20Upgradeable)

- **Signature**: `totalSupply()`
- **Visibility**: public
- **Source Range**: 3877:152:58
- **Details**: [function_totalSupply.md](./function_totalSupply.md)

**Signature:**
```solidity
///  @dev See {IERC20-totalSupply}.
function totalSupply() virtual public view returns (uint256);
```

### balanceOf(address) (inherited from ERC20Upgradeable)

- **Signature**: `balanceOf(address)`
- **Visibility**: public
- **Source Range**: 4087:171:58
- **Details**: [function_balanceOf_address.md](./function_balanceOf_address.md)

**Signature:**
```solidity
///  @dev See {IERC20-balanceOf}.
function balanceOf(address account) virtual public view returns (uint256);
```

### transfer(address,uint256) (inherited from ERC20Upgradeable)

- **Signature**: `transfer(address,uint256)`
- **Visibility**: public
- **Source Range**: 4453:178:58
- **Details**: [function_transfer_address_uint256.md](./function_transfer_address_uint256.md)

**Signature:**
```solidity
///  @dev See {IERC20-transfer}.
///  Requirements:
///  - `to` cannot be the zero address.
///  - the caller must have a balance of at least `value`.
function transfer(address to, uint256 value) virtual public returns (bool);
```

### allowance(address,address) (inherited from ERC20Upgradeable)

- **Signature**: `allowance(address,address)`
- **Visibility**: public
- **Source Range**: 4689:195:58
- **Details**: [function_allowance_address_address.md](./function_allowance_address_address.md)

**Signature:**
```solidity
///  @dev See {IERC20-allowance}.
function allowance(address owner, address spender) virtual public view returns (uint256);
```

### approve(address,uint256) (inherited from ERC20Upgradeable)

- **Signature**: `approve(address,uint256)`
- **Visibility**: public
- **Source Range**: 5191:186:58
- **Details**: [function_approve_address_uint256.md](./function_approve_address_uint256.md)

**Signature:**
```solidity
///  @dev See {IERC20-approve}.
///  NOTE: If `value` is the maximum `uint256`, the allowance is not updated on
///  `transferFrom`. This is semantically equivalent to an infinite approval.
///  Requirements:
///  - `spender` cannot be the zero address.
function approve(address spender, uint256 value) virtual public returns (bool);
```

### transferFrom(address,address,uint256) (inherited from ERC20Upgradeable)

- **Signature**: `transferFrom(address,address,uint256)`
- **Visibility**: public
- **Source Range**: 5969:244:58
- **Details**: [function_transferFrom_address_address_uint256.md](./function_transferFrom_address_address_uint256.md)

**Signature:**
```solidity
///  @dev See {IERC20-transferFrom}.
///  Skips emitting an {Approval} event indicating an allowance update. This is not
///  required by the ERC. See {xref-ERC20-_approve-address-address-uint256-bool-}[_approve].
///  NOTE: Does not update the allowance if the current allowance
///  is the maximum `uint256`.
///  Requirements:
///  - `from` and `to` cannot be the zero address.
///  - `from` must have a balance of at least `value`.
///  - the caller must have allowance for ``from``'s tokens of at least
///  `value`.
function transferFrom(address from, address to, uint256 value) virtual public returns (bool);
```

### asset() (inherited from ERC4626Upgradeable)

- **Signature**: `asset()`
- **Visibility**: public
- **Source Range**: 6882:153:60
- **Details**: [function_asset.md](./function_asset.md)

**Signature:**
```solidity
/// @dev See {IERC4626-asset}. 
function asset() virtual public view returns (address);
```

### convertToShares(uint256) (inherited from ERC4626Upgradeable)

- **Signature**: `convertToShares(uint256)`
- **Visibility**: public
- **Source Range**: 7264:148:60
- **Details**: [function_convertToShares_uint256.md](./function_convertToShares_uint256.md)

**Signature:**
```solidity
/// @dev See {IERC4626-convertToShares}. 
function convertToShares(uint256 assets) virtual public view returns (uint256);
```

### convertToAssets(uint256) (inherited from ERC4626Upgradeable)

- **Signature**: `convertToAssets(uint256)`
- **Visibility**: public
- **Source Range**: 7466:148:60
- **Details**: [function_convertToAssets_uint256.md](./function_convertToAssets_uint256.md)

**Signature:**
```solidity
/// @dev See {IERC4626-convertToAssets}. 
function convertToAssets(uint256 shares) virtual public view returns (uint256);
```

### previewDeposit(uint256) (inherited from ERC4626Upgradeable)

- **Signature**: `previewDeposit(uint256)`
- **Visibility**: public
- **Source Range**: 8338:147:60
- **Details**: [function_previewDeposit_uint256.md](./function_previewDeposit_uint256.md)

**Signature:**
```solidity
/// @dev See {IERC4626-previewDeposit}. 
function previewDeposit(uint256 assets) virtual public view returns (uint256);
```

### previewMint(uint256) (inherited from ERC4626Upgradeable)

- **Signature**: `previewMint(uint256)`
- **Visibility**: public
- **Source Range**: 8535:143:60
- **Details**: [function_previewMint_uint256.md](./function_previewMint_uint256.md)

**Signature:**
```solidity
/// @dev See {IERC4626-previewMint}. 
function previewMint(uint256 shares) virtual public view returns (uint256);
```

### previewWithdraw(uint256) (inherited from ERC4626Upgradeable)

- **Signature**: `previewWithdraw(uint256)`
- **Visibility**: public
- **Source Range**: 8732:147:60
- **Details**: [function_previewWithdraw_uint256.md](./function_previewWithdraw_uint256.md)

**Signature:**
```solidity
/// @dev See {IERC4626-previewWithdraw}. 
function previewWithdraw(uint256 assets) virtual public view returns (uint256);
```

### previewRedeem(uint256) (inherited from ERC4626Upgradeable)

- **Signature**: `previewRedeem(uint256)`
- **Visibility**: public
- **Source Range**: 8931:146:60
- **Details**: [function_previewRedeem_uint256.md](./function_previewRedeem_uint256.md)

**Signature:**
```solidity
/// @dev See {IERC4626-previewRedeem}. 
function previewRedeem(uint256 shares) virtual public view returns (uint256);
```

### deposit(uint256,address) (inherited from ERC4626Upgradeable)

- **Signature**: `deposit(uint256,address)`
- **Visibility**: public
- **Source Range**: 9123:392:60
- **Details**: [function_deposit_uint256_address.md](./function_deposit_uint256_address.md)

**Signature:**
```solidity
/// @dev See {IERC4626-deposit}. 
function deposit(uint256 assets, address receiver) virtual public returns (uint256);
```

### mint(uint256,address) (inherited from ERC4626Upgradeable)

- **Signature**: `mint(uint256,address)`
- **Visibility**: public
- **Source Range**: 9558:380:60
- **Details**: [function_mint_uint256_address.md](./function_mint_uint256_address.md)

**Signature:**
```solidity
/// @dev See {IERC4626-mint}. 
function mint(uint256 shares, address receiver) virtual public returns (uint256);
```

### withdraw(uint256,address,address) (inherited from ERC4626Upgradeable)

- **Signature**: `withdraw(uint256,address,address)`
- **Visibility**: public
- **Source Range**: 9985:413:60
- **Details**: [function_withdraw_uint256_address_address.md](./function_withdraw_uint256_address_address.md)

**Signature:**
```solidity
/// @dev See {IERC4626-withdraw}. 
function withdraw(uint256 assets, address receiver, address owner) virtual public returns (uint256);
```

### redeem(uint256,address,address) (inherited from ERC4626Upgradeable)

- **Signature**: `redeem(uint256,address,address)`
- **Visibility**: public
- **Source Range**: 10443:405:60
- **Details**: [function_redeem_uint256_address_address.md](./function_redeem_uint256_address_address.md)

**Signature:**
```solidity
/// @dev See {IERC4626-redeem}. 
function redeem(uint256 shares, address receiver, address owner) virtual public returns (uint256);
```

### eip712Domain() (inherited from EIP712Upgradeable)

- **Signature**: `eip712Domain()`
- **Visibility**: public
- **Source Range**: 5172:903:66
- **Details**: [function_eip712Domain.md](./function_eip712Domain.md)

**Signature:**
```solidity
///  @inheritdoc IERC5267
function eip712Domain() virtual public view returns (bytes1 fields, string memory name, string memory version, uint256 chainId, address verifyingContract, bytes32 salt, uint256[] memory extensions);
```

### nonces(address) (inherited from NoncesUpgradeable)

- **Signature**: `nonces(address)`
- **Visibility**: public
- **Source Range**: 1259:164:63
- **Details**: [function_nonces_address.md](./function_nonces_address.md)

**Signature:**
```solidity
///  @dev Returns the next unused nonce for an address.
function nonces(address owner) virtual public view returns (uint256);
```

### permit(address,address,uint256,uint256,uint8,bytes32,bytes32) (inherited from ERC20PermitUpgradeable)

- **Signature**: `permit(address,address,uint256,uint256,uint8,bytes32,bytes32)`
- **Visibility**: public
- **Source Range**: 2098:672:59
- **Details**: [function_permit_address_address_uint256_uint256_uint8_bytes32_bytes32.md](./function_permit_address_address_uint256_uint256_uint8_bytes32_bytes32.md)

**Signature:**
```solidity
///  @inheritdoc IERC20Permit
function permit(address owner, address spender, uint256 value, uint256 deadline, uint8 v, bytes32 r, bytes32 s) virtual public;
```

### DOMAIN_SEPARATOR() (inherited from ERC20PermitUpgradeable)

- **Signature**: `DOMAIN_SEPARATOR()`
- **Visibility**: external
- **Source Range**: 3085:112:59
- **Details**: [function_DOMAIN_SEPARATOR.md](./function_DOMAIN_SEPARATOR.md)

**Signature:**
```solidity
///  @inheritdoc IERC20Permit
function DOMAIN_SEPARATOR() virtual external view returns (bytes32);
```

### paused() (inherited from PausableUpgradeable)

- **Signature**: `paused()`
- **Visibility**: public
- **Source Range**: 2496:145:64
- **Details**: [function_paused.md](./function_paused.md)

**Signature:**
```solidity
///  @dev Returns true if the contract is paused, and false otherwise.
function paused() virtual public view returns (bool);
```

### multicall(bytes[]) (inherited from MulticallUpgradeable)

- **Signature**: `multicall(bytes[])`
- **Visibility**: external
- **Source Range**: 1518:484:62
- **Details**: [function_multicall_bytes[].md](./function_multicall_bytes[].md)

**Signature:**
```solidity
///  @dev Receives and executes a batch of function calls on this contract.
///  @custom:oz-upgrades-unsafe-allow-reachable delegatecall
function multicall(bytes[] calldata data) virtual external returns (bytes[] memory results);
```

### proxiableUUID() (inherited from UUPSUpgradeable)

- **Signature**: `proxiableUUID()`
- **Visibility**: external
- **Source Range**: 3708:134:57
- **Details**: [function_proxiableUUID.md](./function_proxiableUUID.md)

**Signature:**
```solidity
///  @dev Implementation of the ERC-1822 {proxiableUUID} function. This returns the storage slot used by the
///  implementation. It is used to validate the implementation's compatibility when performing an upgrade.
///  IMPORTANT: A proxy pointing at a proxiable contract should not be considered proxiable itself, because this risks
///  bricking a proxy that upgrades to it, by delegating to itself until out of gas. Thus it is critical that this
///  function revert if invoked through a proxy. This is guaranteed by the `notDelegated` modifier.
function proxiableUUID() virtual external view notDelegated() returns (bytes32);
```

### upgradeToAndCall(address,bytes) (inherited from UUPSUpgradeable)

- **Signature**: `upgradeToAndCall(address,bytes)`
- **Visibility**: public
- **Source Range**: 4161:214:57
- **Details**: [function_upgradeToAndCall_address_bytes.md](./function_upgradeToAndCall_address_bytes.md)

**Signature:**
```solidity
///  @dev Upgrade the implementation of the proxy to `newImplementation`, and subsequently execute the function call
///  encoded in `data`.
///  Calls {_authorizeUpgrade}.
///  Emits an {Upgraded} event.
///  @custom:oz-upgrades-unsafe-allow-reachable delegatecall
function upgradeToAndCall(address newImplementation, bytes memory data) virtual public payable onlyProxy();
```

### initialize(contract Auth,contract IERC20,string,string,address,uint256) (inherited from BaseVault)

- **Signature**: `initialize(contract Auth,contract IERC20,string,string,address,uint256)`
- **Visibility**: public
- **Source Range**: 3788:820:149
- **Details**: [function_initialize_contract_Auth_contract_IERC20_string_string_address_uint256.md](./function_initialize_contract_Auth_contract_IERC20_string_string_address_uint256.md)

**Signature:**
```solidity
/// @notice Initializes the BaseVault with necessary parameters
///  @param auth_ The address of the Auth contract
///  @param asset_ The address of the asset
///  @param name_ The name of the vault
///  @param symbol_ The symbol of the vault
///  @param fundingAccount_ The address of the funding account for the first deposit, which will be treated as dead shares
///  @param firstDepositAmount_ The amount of the first deposit, which will be treated as dead shares
///  @dev Sets up all inherited contracts and makes the first deposit to prevent inflation attacks
function initialize(Auth auth_, IERC20 asset_, string memory name_, string memory symbol_, address fundingAccount_, uint256 firstDepositAmount_) virtual public initializer();
```

### pause() (inherited from BaseVault)

- **Signature**: `pause()`
- **Visibility**: external
- **Source Range**: 6435:88:149
- **Details**: [function_pause.md](./function_pause.md)

**Signature:**
```solidity
/// @notice Pauses the vault
///  @dev Only addresses with GUARDIAN_ROLE can pause the vault
function pause() external nonReentrant() onlyAuth(GUARDIAN_ROLE);
```

### unpause() (inherited from BaseVault)

- **Signature**: `unpause()`
- **Visibility**: external
- **Source Range**: 6638:97:149
- **Details**: [function_unpause.md](./function_unpause.md)

**Signature:**
```solidity
/// @notice Unpauses the vault
///  @dev Only addresses with VAULT_MANAGER_ROLE can unpause the vault
function unpause() external nonReentrant() onlyAuth(VAULT_MANAGER_ROLE);
```

### setTotalAssetsCap(uint256) (inherited from BaseVault)

- **Signature**: `setTotalAssetsCap(uint256)`
- **Visibility**: external
- **Source Range**: 7015:155:149
- **Details**: [function_setTotalAssetsCap_uint256.md](./function_setTotalAssetsCap_uint256.md)

**Signature:**
```solidity
/// @notice Sets the maximum total assets of the vault
///  @param totalAssetsCap_ The new total assets cap
///  @dev Only addresses with VAULT_MANAGER_ROLE can set the vault cap
///  @dev Lowering the total assets cap does not affect existing deposited assets
function setTotalAssetsCap(uint256 totalAssetsCap_) external nonReentrant() onlyAuth(VAULT_MANAGER_ROLE);
```

### rescueTokens(address,address) (inherited from BaseVault)

- **Signature**: `rescueTokens(address,address)`
- **Visibility**: external
- **Source Range**: 8755:325:149
- **Details**: [function_rescueTokens_address_address.md](./function_rescueTokens_address_address.md)

**Signature:**
```solidity
/// @notice Rescues tokens from the vault
///  @param token The address of the token to rescue
///  @param to The address to send the rescued tokens to
///  @dev Only addresses with GUARDIAN_ROLE can rescue tokens
///  @dev Reverts if the token is the address(0) or the asset of the vault
function rescueTokens(address token, address to) external onlyAuth(GUARDIAN_ROLE);
```

### auth() (inherited from BaseVault)

- **Signature**: `auth()`
- **Visibility**: public
- **Source Range**: 14480:104:149
- **Details**: [function_auth.md](./function_auth.md)

**Signature:**
```solidity
/// @inheritdoc IVault
function auth() override public view returns (Auth);
```

### totalAssetsCap() (inherited from BaseVault)

- **Signature**: `totalAssetsCap()`
- **Visibility**: public
- **Source Range**: 14617:123:149
- **Details**: [function_totalAssetsCap.md](./function_totalAssetsCap.md)

**Signature:**
```solidity
/// @inheritdoc IVault
function totalAssetsCap() override public view nonReentrantView() returns (uint256);
```

### version() (inherited from BaseVault)

- **Signature**: `version()`
- **Visibility**: external
- **Source Range**: 15027:88:149
- **Details**: [function_version.md](./function_version.md)

**Signature:**
```solidity
/// @notice Returns the version of the vault
///  @return The version of the vault
function version() external pure returns (string memory);
```

### highWaterMark() (inherited from PerformanceVault)

- **Signature**: `highWaterMark()`
- **Visibility**: public
- **Source Range**: 7789:112:151
- **Details**: [function_highWaterMark.md](./function_highWaterMark.md)

**Signature:**
```solidity
/// @notice Returns the high water mark
function highWaterMark() public view nonReentrantView() returns (uint256);
```

### performanceFeePercent() (inherited from PerformanceVault)

- **Signature**: `performanceFeePercent()`
- **Visibility**: public
- **Source Range**: 8154:128:151
- **Details**: [function_performanceFeePercent.md](./function_performanceFeePercent.md)

**Signature:**
```solidity
/// @notice Returns the performance fee percent
function performanceFeePercent() public view nonReentrantView() returns (uint256);
```

### feeRecipient() (inherited from PerformanceVault)

- **Signature**: `feeRecipient()`
- **Visibility**: public
- **Source Range**: 8549:110:151
- **Details**: [function_feeRecipient.md](./function_feeRecipient.md)

**Signature:**
```solidity
/// @notice Returns the fee recipient
function feeRecipient() public view nonReentrantView() returns (address);
```
