# Contract: TokenHolder

## Metadata

- **Name**: TokenHolder
- **Type**: Contract
- **Path**: src/TokenHolder.sol

## Implements Interfaces

- **IERC165** [lib/openzeppelin-contracts-upgradeable/lib/openzeppelin-contracts/contracts/utils/introspection/IERC165.sol/interface_IERC165.md]
- **IAccessControl** [lib/openzeppelin-contracts-upgradeable/lib/openzeppelin-contracts/contracts/access/IAccessControl.sol/interface_IAccessControl.md]

## State Variables

### INITIALIZABLE_STORAGE (inherited from Initializable)

```solidity
bytes32 private constant INITIALIZABLE_STORAGE = 0xf0c57e16840df040f15088dc2f81fe391c3923bec73e23a9662efc9c229c6a00
```

### DEFAULT_ADMIN_ROLE (inherited from AccessControlUpgradeable)

```solidity
bytes32 public constant DEFAULT_ADMIN_ROLE = 0x00
```

### AccessControlStorageLocation (inherited from AccessControlUpgradeable)

```solidity
bytes32 private constant AccessControlStorageLocation = 0x02dd7bc7dec4dceedda775e58dd541e08a116c6c53815c0bd028192f7b626800
```

### tokenHolded

```solidity
IERC20 public tokenHolded
```

**IERC20**: [lib/openzeppelin-contracts-upgradeable/lib/openzeppelin-contracts/contracts/token/ERC20/IERC20.sol/interface_IERC20.md]

### collateralMapping

```solidity
mapping(address => Collateral) public collateralMapping
```

### loans

```solidity
mapping(uint256 => Loan) public loans
```

### activeLoanIds

```solidity
uint256[] public activeLoanIds
```

### loanIdToArrayIndex

```solidity
mapping(uint256 => uint256) private loanIdToArrayIndex
```

### nextLoanId

```solidity
uint256 public nextLoanId
```

### BORROWER_ROUTER_ROLE

```solidity
bytes32 public constant BORROWER_ROUTER_ROLE = keccak256("BORROWER_ROUTER_ROLE")
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

### RoleData (inherited from AccessControlUpgradeable)

```solidity
struct RoleData {
    mapping(address => bool) hasRole;
    bytes32 adminRole;
}
```

### AccessControlStorage (inherited from AccessControlUpgradeable)

```solidity
/// @custom:storage-location erc7201:openzeppelin.storage.AccessControl
struct AccessControlStorage {
    mapping(bytes32 => RoleData) _roles;
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

### AccessControlUnauthorizedAccount (inherited from IAccessControl)

```solidity
///  @dev The `account` is missing a role.
error AccessControlUnauthorizedAccount(address account, bytes32 neededRole);
```

### AccessControlBadConfirmation (inherited from IAccessControl)

```solidity
///  @dev The caller of a function is not the expected one.
///  NOTE: Don't confuse with {AccessControlUnauthorizedAccount}.
error AccessControlBadConfirmation();
```

## Events

### Initialized (inherited from Initializable)

```solidity
///  @dev Triggered when the contract has been initialized or reinitialized.
event Initialized(uint64 version);
```

### RoleAdminChanged (inherited from IAccessControl)

```solidity
///  @dev Emitted when `newAdminRole` is set as ``role``'s admin role, replacing `previousAdminRole`
///  `DEFAULT_ADMIN_ROLE` is the starting admin for all roles, despite
///  {RoleAdminChanged} not being emitted to signal this.
event RoleAdminChanged(bytes32 indexed role, bytes32 indexed previousAdminRole, bytes32 indexed newAdminRole);
```

### RoleGranted (inherited from IAccessControl)

```solidity
///  @dev Emitted when `account` is granted `role`.
///  `sender` is the account that originated the contract call. This account bears the admin role (for the granted role).
///  Expected in cases where the role was granted using the internal {AccessControl-_grantRole}.
event RoleGranted(bytes32 indexed role, address indexed account, address indexed sender);
```

### RoleRevoked (inherited from IAccessControl)

```solidity
///  @dev Emitted when `account` is revoked `role`.
///  `sender` is the account that originated the contract call:
///    - if using `revokeRole`, it is the admin role bearer
///    - if using `renounceRole`, it is the role bearer (i.e. `account`)
event RoleRevoked(bytes32 indexed role, address indexed account, address indexed sender);
```

### LoanCreated

```solidity
event LoanCreated(uint256 loanId, uint256 amount, uint256 timestamp, bool repaid);
```

### LoanRepaid

```solidity
event LoanRepaid(uint256 loanId);
```

### PrivilegedLoan

```solidity
event PrivilegedLoan(address borrower, uint256 amount);
```

### PrivilegedLoanRepaid

```solidity
event PrivilegedLoanRepaid(address borrower, uint256 amount);
```

### InterestRateSet

```solidity
event InterestRateSet(uint256 newInterestRate);
```

### ExposureUpdated

```solidity
event ExposureUpdated(address collateralAddress, uint256 currentExposure);
```

### MaxExposureUpdated

```solidity
event MaxExposureUpdated(address collateralAddress, uint256 oldMaxExposure, uint256 newMaxExposure);
```

### MaxLendPerTokenUpdated

```solidity
event MaxLendPerTokenUpdated(address collateralAddress, uint256 oldValue, uint256 newValue);
```

## Public/External Functions

### initialize(address)

- **Signature**: `initialize(address)`
- **Visibility**: public
- **Source Range**: 1911:166:13
- **Details**: [function_initialize_address.md](./function_initialize_address.md)

**Signature:**
```solidity
function initialize(address _tokenAddress) public initializer();
```

### deposit(uint256)

- **Signature**: `deposit(uint256)`
- **Visibility**: public
- **Source Range**: 2322:275:13
- **Details**: [function_deposit_uint256.md](./function_deposit_uint256.md)

**Signature:**
```solidity
function deposit(uint256 amount) public onlyRole(DEFAULT_ADMIN_ROLE);
```

### withdraw(uint256)

- **Signature**: `withdraw(uint256)`
- **Visibility**: public
- **Source Range**: 2644:223:13
- **Details**: [function_withdraw_uint256.md](./function_withdraw_uint256.md)

**Signature:**
```solidity
function withdraw(uint256 amount) public onlyRole(DEFAULT_ADMIN_ROLE);
```

### getBalance()

- **Signature**: `getBalance()`
- **Visibility**: public
- **Source Range**: 2913:112:13
- **Details**: [function_getBalance.md](./function_getBalance.md)

**Signature:**
```solidity
function getBalance() public view returns (uint256);
```

### loanConfirmation(uint256,uint256,address,address,uint256)

- **Signature**: `loanConfirmation(uint256,uint256,address,address,uint256)`
- **Visibility**: public
- **Source Range**: 3072:1777:13
- **Details**: [function_loanConfirmation_uint256_uint256_address_address_uint256.md](./function_loanConfirmation_uint256_uint256_address_address_uint256.md)

**Signature:**
```solidity
function loanConfirmation(uint256 amount, uint256 collateralAmount, address collateralAddress, address borrower, uint256 userPaid) public onlyBorrowerRouter() returns (uint256);
```

### repayLoan(uint256,bool)

- **Signature**: `repayLoan(uint256,bool)`
- **Visibility**: public
- **Source Range**: 4912:2144:13
- **Details**: [function_repayLoan_uint256_bool.md](./function_repayLoan_uint256_bool.md)

**Signature:**
```solidity
function repayLoan(uint256 loanId, bool withoutTransfer) public onlyBorrowerRouter();
```

### getActiveLoanCount()

- **Signature**: `getActiveLoanCount()`
- **Visibility**: public
- **Source Range**: 7106:104:13
- **Details**: [function_getActiveLoanCount.md](./function_getActiveLoanCount.md)

**Signature:**
```solidity
function getActiveLoanCount() public view returns (uint256);
```

### getActiveLoansBatch(uint256,uint256)

- **Signature**: `getActiveLoansBatch(uint256,uint256)`
- **Visibility**: public
- **Source Range**: 7292:613:13
- **Details**: [function_getActiveLoansBatch_uint256_uint256.md](./function_getActiveLoansBatch_uint256_uint256.md)

**Signature:**
```solidity
function getActiveLoansBatch(uint256 startIndex, uint256 batchSize) public view returns (Loan[] memory);
```

### getLoansByBorrower(address)

- **Signature**: `getLoansByBorrower(address)`
- **Visibility**: public
- **Source Range**: 7972:828:13
- **Details**: [function_getLoansByBorrower_address.md](./function_getLoansByBorrower_address.md)

**Signature:**
```solidity
function getLoansByBorrower(address borrower) public view returns (Loan[] memory);
```

### privilegedLoan(contract IERC20,uint256)

- **Signature**: `privilegedLoan(contract IERC20,uint256)`
- **Visibility**: public
- **Source Range**: 8886:492:13
- **Details**: [function_privilegedLoan_contract_IERC20_uint256.md](./function_privilegedLoan_contract_IERC20_uint256.md)

**Signature:**
```solidity
function privilegedLoan(IERC20 flashLoanToken, uint256 amount) public onlyBorrowerRouter();
```

### grantBorrowerRouterRole(address)

- **Signature**: `grantBorrowerRouterRole(address)`
- **Visibility**: public
- **Source Range**: 9435:158:13
- **Details**: [function_grantBorrowerRouterRole_address.md](./function_grantBorrowerRouterRole_address.md)

**Signature:**
```solidity
function grantBorrowerRouterRole(address account) public onlyRole(DEFAULT_ADMIN_ROLE);
```

### revokeBorrowerRouterRole(address)

- **Signature**: `revokeBorrowerRouterRole(address)`
- **Visibility**: public
- **Source Range**: 9653:160:13
- **Details**: [function_revokeBorrowerRouterRole_address.md](./function_revokeBorrowerRouterRole_address.md)

**Signature:**
```solidity
function revokeBorrowerRouterRole(address account) public onlyRole(DEFAULT_ADMIN_ROLE);
```

### addCollateral(address,uint256,uint256,uint256,uint256)

- **Signature**: `addCollateral(address,uint256,uint256,uint256,uint256)`
- **Visibility**: public
- **Source Range**: 9868:731:13
- **Details**: [function_addCollateral_address_uint256_uint256_uint256_uint256.md](./function_addCollateral_address_uint256_uint256_uint256_uint256.md)

**Signature:**
```solidity
function addCollateral(address collateralAddress, uint256 _interestRate, uint256 maxLendPerToken, uint256 minAmount, uint256 maxExposure) public onlyRole(DEFAULT_ADMIN_ROLE);
```

### updateMaxExposure(address,uint256)

- **Signature**: `updateMaxExposure(address,uint256)`
- **Visibility**: public
- **Source Range**: 10669:613:13
- **Details**: [function_updateMaxExposure_address_uint256.md](./function_updateMaxExposure_address_uint256.md)

**Signature:**
```solidity
function updateMaxExposure(address collateralAddress, uint256 newMaxExposure) public onlyRole(DEFAULT_ADMIN_ROLE);
```

### getCollateralExposure(address)

- **Signature**: `getCollateralExposure(address)`
- **Visibility**: public
- **Source Range**: 11345:164:13
- **Details**: [function_getCollateralExposure_address.md](./function_getCollateralExposure_address.md)

**Signature:**
```solidity
function getCollateralExposure(address collateralAddress) public view returns (uint256);
```

### getAvailableExposure(address)

- **Signature**: `getAvailableExposure(address)`
- **Visibility**: public
- **Source Range**: 11574:355:13
- **Details**: [function_getAvailableExposure_address.md](./function_getAvailableExposure_address.md)

**Signature:**
```solidity
function getAvailableExposure(address collateralAddress) public view returns (uint256);
```

### updateMaxLendPerTokenBulk(address[],uint256[])

- **Signature**: `updateMaxLendPerTokenBulk(address[],uint256[])`
- **Visibility**: public
- **Source Range**: 12184:1051:13
- **Details**: [function_updateMaxLendPerTokenBulk_address[]_uint256[].md](./function_updateMaxLendPerTokenBulk_address[]_uint256[].md)

**Signature:**
```solidity
function updateMaxLendPerTokenBulk(address[] memory collateralAddresses, uint256[] memory newMaxLendPerTokens) public onlyRole(DEFAULT_ADMIN_ROLE);
```

### removeCollateral(address)

- **Signature**: `removeCollateral(address)`
- **Visibility**: public
- **Source Range**: 13279:448:13
- **Details**: [function_removeCollateral_address.md](./function_removeCollateral_address.md)

**Signature:**
```solidity
function removeCollateral(address collateralAddress) public onlyRole(DEFAULT_ADMIN_ROLE);
```

### supportsInterface(bytes4) (inherited from ERC165Upgradeable)

- **Signature**: `supportsInterface(bytes4)`
- **Visibility**: public
- **Source Range**: 1020:146:4
- **Details**: [function_supportsInterface_bytes4.md](./function_supportsInterface_bytes4.md)

**Signature:**
```solidity
/// @inheritdoc IERC165
function supportsInterface(bytes4 interfaceId) virtual public view returns (bool);
```

### hasRole(bytes32,address) (inherited from AccessControlUpgradeable)

- **Signature**: `hasRole(bytes32,address)`
- **Visibility**: public
- **Source Range**: 3801:207:0
- **Details**: [function_hasRole_bytes32_address.md](./function_hasRole_bytes32_address.md)

**Signature:**
```solidity
///  @dev Returns `true` if `account` has been granted `role`.
function hasRole(bytes32 role, address account) virtual public view returns (bool);
```

### getRoleAdmin(bytes32) (inherited from AccessControlUpgradeable)

- **Signature**: `getRoleAdmin(bytes32)`
- **Visibility**: public
- **Source Range**: 4828:191:0
- **Details**: [function_getRoleAdmin_bytes32.md](./function_getRoleAdmin_bytes32.md)

**Signature:**
```solidity
///  @dev Returns the admin role that controls `role`. See {grantRole} and
///  {revokeRole}.
///  To change a role's admin, use {_setRoleAdmin}.
function getRoleAdmin(bytes32 role) virtual public view returns (bytes32);
```

### grantRole(bytes32,address) (inherited from AccessControlUpgradeable)

- **Signature**: `grantRole(bytes32,address)`
- **Visibility**: public
- **Source Range**: 5315:136:0
- **Details**: [function_grantRole_bytes32_address.md](./function_grantRole_bytes32_address.md)

**Signature:**
```solidity
///  @dev Grants `role` to `account`.
///  If `account` had not been already granted `role`, emits a {RoleGranted}
///  event.
///  Requirements:
///  - the caller must have ``role``'s admin role.
///  May emit a {RoleGranted} event.
function grantRole(bytes32 role, address account) virtual public onlyRole(getRoleAdmin(role));
```

### revokeRole(bytes32,address) (inherited from AccessControlUpgradeable)

- **Signature**: `revokeRole(bytes32,address)`
- **Visibility**: public
- **Source Range**: 5731:138:0
- **Details**: [function_revokeRole_bytes32_address.md](./function_revokeRole_bytes32_address.md)

**Signature:**
```solidity
///  @dev Revokes `role` from `account`.
///  If `account` had been granted `role`, emits a {RoleRevoked} event.
///  Requirements:
///  - the caller must have ``role``'s admin role.
///  May emit a {RoleRevoked} event.
function revokeRole(bytes32 role, address account) virtual public onlyRole(getRoleAdmin(role));
```

### renounceRole(bytes32,address) (inherited from AccessControlUpgradeable)

- **Signature**: `renounceRole(bytes32,address)`
- **Visibility**: public
- **Source Range**: 6417:245:0
- **Details**: [function_renounceRole_bytes32_address.md](./function_renounceRole_bytes32_address.md)

**Signature:**
```solidity
///  @dev Revokes `role` from the calling account.
///  Roles are often managed via {grantRole} and {revokeRole}: this function's
///  purpose is to provide a mechanism for accounts to lose their privileges
///  if they are compromised (such as when a trusted device is misplaced).
///  If the calling account had been revoked `role`, emits a {RoleRevoked}
///  event.
///  Requirements:
///  - the caller must be `callerConfirmation`.
///  May emit a {RoleRevoked} event.
function renounceRole(bytes32 role, address callerConfirmation) virtual public;
```
