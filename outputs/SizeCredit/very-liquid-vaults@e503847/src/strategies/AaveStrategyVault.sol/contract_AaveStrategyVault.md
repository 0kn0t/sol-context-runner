# Contract: AaveStrategyVault

## Metadata

- **Name**: AaveStrategyVault
- **Type**: Contract
- **Path**: src/strategies/AaveStrategyVault.sol
- **Documentation**: @title AaveStrategyVault
   @custom:security-contact security@size.credit
   @author Size (https://size.credit/)
   @notice A strategy that invests assets in Aave v3 lending pools
   @dev Extends BaseVault for Aave v3 integration within the Size Meta Vault system
   @dev Reference https://github.com/superform-xyz/super-vaults/blob/8bc1d1bd1579f6fb9a047802256ed3a2bf15f602/src/aave-v3/AaveV3ERC4626Reinvest.sol

## Implements Interfaces

- **IERC1822Proxiable** [lib/openzeppelin-contracts/contracts/interfaces/draft-IERC1822.sol/interface_IERC1822Proxiable.md]
- **IERC5267** [lib/openzeppelin-contracts/contracts/interfaces/IERC5267.sol/interface_IERC5267.md]
- **IERC20Permit** [lib/openzeppelin-contracts/contracts/token/ERC20/extensions/IERC20Permit.sol/interface_IERC20Permit.md]
- **IBaseVault** [src/IBaseVault.sol/interface_IBaseVault.md]
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

### auth (inherited from BaseVault)

```solidity
Auth public auth
```

**Auth**: [src/utils/Auth.sol/contract_Auth.md]

### deadAssets (inherited from BaseVault)

```solidity
uint256 public deadAssets
```

### totalAssetsCap (inherited from BaseVault)

```solidity
uint256 public totalAssetsCap
```

### __gap (inherited from BaseVault)

```solidity
uint256[47] private __gap
```

### pool

```solidity
IPool public pool
```

**IPool**: [lib/aave-v3-origin/src/contracts/interfaces/IPool.sol/interface_IPool.md]

### aToken

```solidity
IAToken public aToken
```

**IAToken**: [lib/aave-v3-origin/src/contracts/interfaces/IAToken.sol/interface_IAToken.md]

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

### Skim (inherited from IBaseVault)

```solidity
/// @notice Emitted when the vault skims idle assets and re-invests them in the underlying protocol
event Skim();
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

### DeadAssetsSet (inherited from BaseVault)

```solidity
event DeadAssetsSet(uint256 indexed deadAssets);
```

### TotalAssetsCapSet (inherited from BaseVault)

```solidity
event TotalAssetsCapSet(uint256 indexed totalAssetsCapBefore, uint256 indexed totalAssetsCapAfter);
```

### VaultStatus (inherited from BaseVault)

```solidity
event VaultStatus(uint256 indexed timestamp, uint256 totalShares, uint256 totalAssets);
```

### PoolSet

```solidity
event PoolSet(address indexed pool);
```

### ATokenSet

```solidity
event ATokenSet(address indexed aToken);
```

## Public/External Functions

### initialize(contract Auth,contract IERC20,string,string,address,uint256,contract IPool)

- **Signature**: `initialize(contract Auth,contract IERC20,string,string,address,uint256,contract IPool)`
- **Visibility**: public
- **Source Range**: 2396:765:60
- **Details**: [function_initialize_contract_Auth_contract_IERC20_string_string_address_uint256_contract_IPool.md](./function_initialize_contract_Auth_contract_IERC20_string_string_address_uint256_contract_IPool.md)

**Signature:**
```solidity
/// @notice Initializes the AaveStrategyVault with an Aave pool
///  @dev Sets the Aave pool and retrieves the corresponding aToken address
function initialize(Auth auth_, IERC20 asset_, string memory name_, string memory symbol_, address fundingAccount, uint256 firstDepositAmount, IPool pool_) virtual public initializer();
```

### skim()

- **Signature**: `skim()`
- **Visibility**: external
- **Source Range**: 3492:269:60
- **Details**: [function_skim.md](./function_skim.md)

**Signature:**
```solidity
/// @notice Invests any idle assets sitting in this contract
///  @dev Supplies any assets held by this contract to the Aave pool
function skim() override external nonReentrant() notPaused();
```

### maxDeposit(address)

- **Signature**: `maxDeposit(address)`
- **Visibility**: public
- **Source Range**: 4348:1009:60
- **Details**: [function_maxDeposit_address.md](./function_maxDeposit_address.md)

**Signature:**
```solidity
/// @notice Returns the maximum amount that can be deposited
///  @dev Checks Aave reserve configuration and supply cap to determine max deposit
///  @dev Updates Superform implementation to comply with https://github.com/aave-dao/aave-v3-origin/blob/v3.4.0/src/contracts/protocol/libraries/logic/ValidationLogic.sol#L79-L85
///  @return The maximum deposit amount allowed by Aave
function maxDeposit(address receiver) override(BaseVault) public view returns (uint256);
```

### maxMint(address)

- **Signature**: `maxMint(address)`
- **Visibility**: public
- **Source Range**: 5490:181:60
- **Details**: [function_maxMint_address.md](./function_maxMint_address.md)

**Signature:**
```solidity
/// @notice Returns the maximum number of shares that can be minted
///  @dev Converts the max deposit amount to shares
function maxMint(address receiver) override(BaseVault) public view returns (uint256);
```

### maxWithdraw(address)

- **Signature**: `maxWithdraw(address)`
- **Visibility**: public
- **Source Range**: 5823:537:60
- **Details**: [function_maxWithdraw_address.md](./function_maxWithdraw_address.md)

**Signature:**
```solidity
/// @notice Returns the maximum amount that can be withdrawn by an owner
///  @dev Limited by both owner's balance and Aave pool liquidity
function maxWithdraw(address owner) override(ERC4626Upgradeable, IERC4626) public view returns (uint256);
```

### maxRedeem(address)

- **Signature**: `maxRedeem(address)`
- **Visibility**: public
- **Source Range**: 6596:585:60
- **Details**: [function_maxRedeem_address.md](./function_maxRedeem_address.md)

**Signature:**
```solidity
/// @notice Returns the maximum number of shares that can be redeemed
///  @dev Updates Superform implementation to allow the SizeMetaVault to redeem all
///  @dev Limited by both owner's balance and Aave pool liquidity
function maxRedeem(address owner) override(ERC4626Upgradeable, IERC4626) public view returns (uint256);
```

### totalAssets()

- **Signature**: `totalAssets()`
- **Visibility**: public
- **Source Range**: 7481:398:60
- **Details**: [function_totalAssets.md](./function_totalAssets.md)

**Signature:**
```solidity
/// @notice Returns the total assets managed by this strategy
///  @dev Returns the aToken balance since aTokens represent the underlying asset with accrued interest
///  @dev Round down to avoid stealing assets in roundtrip operations https://github.com/a16z/erc4626-tests/issues/13
function totalAssets() virtual override(ERC4626Upgradeable, IERC4626) public view returns (uint256);
```

### name() (inherited from ERC20Upgradeable)

- **Signature**: `name()`
- **Visibility**: public
- **Source Range**: 2697:144:15
- **Details**: [function_name.md](./function_name.md)

**Signature:**
```solidity
///  @dev Returns the name of the token.
function name() virtual public view returns (string memory);
```

### symbol() (inherited from ERC20Upgradeable)

- **Signature**: `symbol()`
- **Visibility**: public
- **Source Range**: 2954:148:15
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
- **Source Range**: 3735:82:15
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
- **Source Range**: 3877:152:15
- **Details**: [function_totalSupply.md](./function_totalSupply.md)

**Signature:**
```solidity
///  @dev See {IERC20-totalSupply}.
function totalSupply() virtual public view returns (uint256);
```

### balanceOf(address) (inherited from ERC20Upgradeable)

- **Signature**: `balanceOf(address)`
- **Visibility**: public
- **Source Range**: 4087:171:15
- **Details**: [function_balanceOf_address.md](./function_balanceOf_address.md)

**Signature:**
```solidity
///  @dev See {IERC20-balanceOf}.
function balanceOf(address account) virtual public view returns (uint256);
```

### transfer(address,uint256) (inherited from ERC20Upgradeable)

- **Signature**: `transfer(address,uint256)`
- **Visibility**: public
- **Source Range**: 4453:178:15
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
- **Source Range**: 4689:195:15
- **Details**: [function_allowance_address_address.md](./function_allowance_address_address.md)

**Signature:**
```solidity
///  @dev See {IERC20-allowance}.
function allowance(address owner, address spender) virtual public view returns (uint256);
```

### approve(address,uint256) (inherited from ERC20Upgradeable)

- **Signature**: `approve(address,uint256)`
- **Visibility**: public
- **Source Range**: 5191:186:15
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
- **Source Range**: 5969:244:15
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
- **Source Range**: 6882:153:17
- **Details**: [function_asset.md](./function_asset.md)

**Signature:**
```solidity
/// @dev See {IERC4626-asset}. 
function asset() virtual public view returns (address);
```

### convertToShares(uint256) (inherited from ERC4626Upgradeable)

- **Signature**: `convertToShares(uint256)`
- **Visibility**: public
- **Source Range**: 7264:148:17
- **Details**: [function_convertToShares_uint256.md](./function_convertToShares_uint256.md)

**Signature:**
```solidity
/// @dev See {IERC4626-convertToShares}. 
function convertToShares(uint256 assets) virtual public view returns (uint256);
```

### convertToAssets(uint256) (inherited from ERC4626Upgradeable)

- **Signature**: `convertToAssets(uint256)`
- **Visibility**: public
- **Source Range**: 7466:148:17
- **Details**: [function_convertToAssets_uint256.md](./function_convertToAssets_uint256.md)

**Signature:**
```solidity
/// @dev See {IERC4626-convertToAssets}. 
function convertToAssets(uint256 shares) virtual public view returns (uint256);
```

### previewDeposit(uint256) (inherited from ERC4626Upgradeable)

- **Signature**: `previewDeposit(uint256)`
- **Visibility**: public
- **Source Range**: 8338:147:17
- **Details**: [function_previewDeposit_uint256.md](./function_previewDeposit_uint256.md)

**Signature:**
```solidity
/// @dev See {IERC4626-previewDeposit}. 
function previewDeposit(uint256 assets) virtual public view returns (uint256);
```

### previewMint(uint256) (inherited from ERC4626Upgradeable)

- **Signature**: `previewMint(uint256)`
- **Visibility**: public
- **Source Range**: 8535:143:17
- **Details**: [function_previewMint_uint256.md](./function_previewMint_uint256.md)

**Signature:**
```solidity
/// @dev See {IERC4626-previewMint}. 
function previewMint(uint256 shares) virtual public view returns (uint256);
```

### previewWithdraw(uint256) (inherited from ERC4626Upgradeable)

- **Signature**: `previewWithdraw(uint256)`
- **Visibility**: public
- **Source Range**: 8732:147:17
- **Details**: [function_previewWithdraw_uint256.md](./function_previewWithdraw_uint256.md)

**Signature:**
```solidity
/// @dev See {IERC4626-previewWithdraw}. 
function previewWithdraw(uint256 assets) virtual public view returns (uint256);
```

### previewRedeem(uint256) (inherited from ERC4626Upgradeable)

- **Signature**: `previewRedeem(uint256)`
- **Visibility**: public
- **Source Range**: 8931:146:17
- **Details**: [function_previewRedeem_uint256.md](./function_previewRedeem_uint256.md)

**Signature:**
```solidity
/// @dev See {IERC4626-previewRedeem}. 
function previewRedeem(uint256 shares) virtual public view returns (uint256);
```

### deposit(uint256,address) (inherited from ERC4626Upgradeable)

- **Signature**: `deposit(uint256,address)`
- **Visibility**: public
- **Source Range**: 9123:392:17
- **Details**: [function_deposit_uint256_address.md](./function_deposit_uint256_address.md)

**Signature:**
```solidity
/// @dev See {IERC4626-deposit}. 
function deposit(uint256 assets, address receiver) virtual public returns (uint256);
```

### mint(uint256,address) (inherited from ERC4626Upgradeable)

- **Signature**: `mint(uint256,address)`
- **Visibility**: public
- **Source Range**: 9558:380:17
- **Details**: [function_mint_uint256_address.md](./function_mint_uint256_address.md)

**Signature:**
```solidity
/// @dev See {IERC4626-mint}. 
function mint(uint256 shares, address receiver) virtual public returns (uint256);
```

### withdraw(uint256,address,address) (inherited from ERC4626Upgradeable)

- **Signature**: `withdraw(uint256,address,address)`
- **Visibility**: public
- **Source Range**: 9985:413:17
- **Details**: [function_withdraw_uint256_address_address.md](./function_withdraw_uint256_address_address.md)

**Signature:**
```solidity
/// @dev See {IERC4626-withdraw}. 
function withdraw(uint256 assets, address receiver, address owner) virtual public returns (uint256);
```

### redeem(uint256,address,address) (inherited from ERC4626Upgradeable)

- **Signature**: `redeem(uint256,address,address)`
- **Visibility**: public
- **Source Range**: 10443:405:17
- **Details**: [function_redeem_uint256_address_address.md](./function_redeem_uint256_address_address.md)

**Signature:**
```solidity
/// @dev See {IERC4626-redeem}. 
function redeem(uint256 shares, address receiver, address owner) virtual public returns (uint256);
```

### eip712Domain() (inherited from EIP712Upgradeable)

- **Signature**: `eip712Domain()`
- **Visibility**: public
- **Source Range**: 5172:903:23
- **Details**: [function_eip712Domain.md](./function_eip712Domain.md)

**Signature:**
```solidity
///  @inheritdoc IERC5267
function eip712Domain() virtual public view returns (bytes1 fields, string memory name, string memory version, uint256 chainId, address verifyingContract, bytes32 salt, uint256[] memory extensions);
```

### nonces(address) (inherited from NoncesUpgradeable)

- **Signature**: `nonces(address)`
- **Visibility**: public
- **Source Range**: 1259:164:20
- **Details**: [function_nonces_address.md](./function_nonces_address.md)

**Signature:**
```solidity
///  @dev Returns the next unused nonce for an address.
function nonces(address owner) virtual public view returns (uint256);
```

### permit(address,address,uint256,uint256,uint8,bytes32,bytes32) (inherited from ERC20PermitUpgradeable)

- **Signature**: `permit(address,address,uint256,uint256,uint8,bytes32,bytes32)`
- **Visibility**: public
- **Source Range**: 2098:672:16
- **Details**: [function_permit_address_address_uint256_uint256_uint8_bytes32_bytes32.md](./function_permit_address_address_uint256_uint256_uint8_bytes32_bytes32.md)

**Signature:**
```solidity
///  @inheritdoc IERC20Permit
function permit(address owner, address spender, uint256 value, uint256 deadline, uint8 v, bytes32 r, bytes32 s) virtual public;
```

### DOMAIN_SEPARATOR() (inherited from ERC20PermitUpgradeable)

- **Signature**: `DOMAIN_SEPARATOR()`
- **Visibility**: external
- **Source Range**: 3085:112:16
- **Details**: [function_DOMAIN_SEPARATOR.md](./function_DOMAIN_SEPARATOR.md)

**Signature:**
```solidity
///  @inheritdoc IERC20Permit
function DOMAIN_SEPARATOR() virtual external view returns (bytes32);
```

### paused() (inherited from PausableUpgradeable)

- **Signature**: `paused()`
- **Visibility**: public
- **Source Range**: 2496:145:21
- **Details**: [function_paused.md](./function_paused.md)

**Signature:**
```solidity
///  @dev Returns true if the contract is paused, and false otherwise.
function paused() virtual public view returns (bool);
```

### multicall(bytes[]) (inherited from MulticallUpgradeable)

- **Signature**: `multicall(bytes[])`
- **Visibility**: external
- **Source Range**: 1518:484:19
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
- **Source Range**: 3708:134:14
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
- **Source Range**: 4161:214:14
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
- **Source Range**: 3411:896:56
- **Details**: [function_initialize_contract_Auth_contract_IERC20_string_string_address_uint256.md](./function_initialize_contract_Auth_contract_IERC20_string_string_address_uint256.md)

**Signature:**
```solidity
/// @notice Initializes the BaseVault with necessary parameters
///  @dev Sets up all inherited contracts and makes the first deposit to prevent inflation attacks
function initialize(Auth auth_, IERC20 asset_, string memory name_, string memory symbol_, address fundingAccount_, uint256 firstDepositAmount_) virtual public initializer();
```

### pause() (inherited from BaseVault)

- **Signature**: `pause()`
- **Visibility**: external
- **Source Range**: 5595:73:56
- **Details**: [function_pause.md](./function_pause.md)

**Signature:**
```solidity
/// @notice Pauses the vault
///  @dev Only addresses with PAUSER_ROLE can pause the vault
function pause() external onlyAuth(PAUSER_ROLE);
```

### unpause() (inherited from BaseVault)

- **Signature**: `unpause()`
- **Visibility**: external
- **Source Range**: 5776:77:56
- **Details**: [function_unpause.md](./function_unpause.md)

**Signature:**
```solidity
/// @notice Unpauses the vault
///  @dev Only addresses with PAUSER_ROLE can unpause the vault
function unpause() external onlyAuth(PAUSER_ROLE);
```

### setTotalAssetsCap(uint256) (inherited from BaseVault)

- **Signature**: `setTotalAssetsCap(uint256)`
- **Visibility**: external
- **Source Range**: 6051:139:56
- **Details**: [function_setTotalAssetsCap_uint256.md](./function_setTotalAssetsCap_uint256.md)

**Signature:**
```solidity
/// @notice Sets the maximum total assets of the vault
///  @dev Only callable by the auth contract
///  @dev Lowering the total assets cap does not affect existing deposited assets
function setTotalAssetsCap(uint256 totalAssetsCap_) external onlyAuth(STRATEGIST_ROLE);
```
