# Contract: BorrowerOperationsV6

## Metadata

- **Name**: BorrowerOperationsV6
- **Type**: Contract
- **Path**: src/BorrowerOperationsV6.sol
- **Documentation**: @custom:oz-upgrades-from src/BorrowerOperationsV5.sol:BorrowerOperationsV5

## State Variables

### INITIALIZABLE_STORAGE (inherited from Initializable)

```solidity
bytes32 private constant INITIALIZABLE_STORAGE = 0xf0c57e16840df040f15088dc2f81fe391c3923bec73e23a9662efc9c229c6a00
```

### REENTRANCY_GUARD_STORAGE (inherited from ReentrancyGuardUpgradeable)

```solidity
bytes32 private constant REENTRANCY_GUARD_STORAGE = 0x9b779b17422d0df92223018b32b4d1fa46e071723d6817e2486d003becc55f00
```

### NOT_ENTERED (inherited from ReentrancyGuardUpgradeable)

```solidity
uint256 private constant NOT_ENTERED = 1
```

### ENTERED (inherited from ReentrancyGuardUpgradeable)

```solidity
uint256 private constant ENTERED = 2
```

### MAX_INT

```solidity
uint256 internal constant MAX_INT = 0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff
```

### FEE_ADDRESS

```solidity
address internal constant FEE_ADDRESS = 0x8432CD30C4d72Ee793399E274C482223DCA2bF9e
```

### weth

```solidity
IERC20 internal weth
```

**IERC20**: [lib/openzeppelin-contracts-upgradeable/lib/openzeppelin-contracts/contracts/token/ERC20/IERC20.sol/interface_IERC20.md]

### openingFee

```solidity
uint256 public openingFee
```

### profitFee

```solidity
uint256 public profitFee
```

### admin

```solidity
address public admin
```

### loanRecords

```solidity
mapping(address => LoanRecord[]) public loanRecords
```

### whitelistedDexes

```solidity
mapping(address => bool) public whitelistedDexes
```

### whitelistedTokens

```solidity
mapping(address => bool) public whitelistedTokens
```

### maxApprovalAmount

```solidity
uint256 public maxApprovalAmount
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

### ReentrancyGuardStorage (inherited from ReentrancyGuardUpgradeable)

```solidity
/// @custom:storage-location erc7201:openzeppelin.storage.ReentrancyGuard
struct ReentrancyGuardStorage {
    uint256 _status;
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

### ReentrancyGuardReentrantCall (inherited from ReentrancyGuardUpgradeable)

```solidity
///  @dev Unauthorized reentrant call.
error ReentrancyGuardReentrantCall();
```

## Events

### Initialized (inherited from Initializable)

```solidity
///  @dev Triggered when the contract has been initialized or reinitialized.
event Initialized(uint64 version);
```

### Buy

```solidity
event Buy(address indexed buyer, address indexed tokenHolder, address indexed tokenCollateral, uint256 loanId, uint256 openingPositionSize, uint256 collateralAmount, uint256 initialMargin);
```

### Sell

```solidity
event Sell(address indexed buyer, address indexed tokenHolder, address indexed tokenCollateral, uint256 loanId, uint256 closingPositionSize, uint256 profit);
```

### Liquidation

```solidity
event Liquidation(address indexed borrower, address indexed tokenHolder, address indexed tokenCollateral, uint256 loanId, uint256 closingPositionSize, uint256 liquidatorRepaidAmount);
```

### DexAdded

```solidity
event DexAdded(address indexed dex);
```

### DexRemoved

```solidity
event DexRemoved(address indexed dex);
```

### TokenAdded

```solidity
event TokenAdded(address indexed token);
```

### TokenRemoved

```solidity
event TokenRemoved(address indexed token);
```

### MaxApprovalAmountUpdated

```solidity
event MaxApprovalAmountUpdated(uint256 oldAmount, uint256 newAmount);
```

## Public/External Functions

### initialize(contract IERC20)

- **Signature**: `initialize(contract IERC20)`
- **Visibility**: public
- **Source Range**: 1899:344:12
- **Details**: [function_initialize_contract_IERC20.md](./function_initialize_contract_IERC20.md)

**Signature:**
```solidity
function initialize(IERC20 _weth) public initializer();
```

### setAdmin(address)

- **Signature**: `setAdmin(address)`
- **Visibility**: external
- **Source Range**: 2369:84:12
- **Details**: [function_setAdmin_address.md](./function_setAdmin_address.md)

**Signature:**
```solidity
function setAdmin(address _admin) external onlyAdmin();
```

### setOpeningFee(uint256)

- **Signature**: `setOpeningFee(uint256)`
- **Visibility**: external
- **Source Range**: 2459:104:12
- **Details**: [function_setOpeningFee_uint256.md](./function_setOpeningFee_uint256.md)

**Signature:**
```solidity
function setOpeningFee(uint256 _openingFee) external onlyAdmin();
```

### setProfitFee(uint256)

- **Signature**: `setProfitFee(uint256)`
- **Visibility**: external
- **Source Range**: 2569:100:12
- **Details**: [function_setProfitFee_uint256.md](./function_setProfitFee_uint256.md)

**Signature:**
```solidity
function setProfitFee(uint256 _profitFee) external onlyAdmin();
```

### addWhitelistedDex(address)

- **Signature**: `addWhitelistedDex(address)`
- **Visibility**: external
- **Source Range**: 2717:191:12
- **Details**: [function_addWhitelistedDex_address.md](./function_addWhitelistedDex_address.md)

**Signature:**
```solidity
function addWhitelistedDex(address dex) external onlyAdmin();
```

### removeWhitelistedDex(address)

- **Signature**: `removeWhitelistedDex(address)`
- **Visibility**: external
- **Source Range**: 2914:138:12
- **Details**: [function_removeWhitelistedDex_address.md](./function_removeWhitelistedDex_address.md)

**Signature:**
```solidity
function removeWhitelistedDex(address dex) external onlyAdmin();
```

### isWhitelistedDex(address)

- **Signature**: `isWhitelistedDex(address)`
- **Visibility**: external
- **Source Range**: 3058:113:12
- **Details**: [function_isWhitelistedDex_address.md](./function_isWhitelistedDex_address.md)

**Signature:**
```solidity
function isWhitelistedDex(address dex) external view returns (bool);
```

### addWhitelistedToken(address)

- **Signature**: `addWhitelistedToken(address)`
- **Visibility**: external
- **Source Range**: 3221:206:12
- **Details**: [function_addWhitelistedToken_address.md](./function_addWhitelistedToken_address.md)

**Signature:**
```solidity
function addWhitelistedToken(address token) external onlyAdmin();
```

### removeWhitelistedToken(address)

- **Signature**: `removeWhitelistedToken(address)`
- **Visibility**: external
- **Source Range**: 3433:149:12
- **Details**: [function_removeWhitelistedToken_address.md](./function_removeWhitelistedToken_address.md)

**Signature:**
```solidity
function removeWhitelistedToken(address token) external onlyAdmin();
```

### isWhitelistedToken(address)

- **Signature**: `isWhitelistedToken(address)`
- **Visibility**: external
- **Source Range**: 3588:120:12
- **Details**: [function_isWhitelistedToken_address.md](./function_isWhitelistedToken_address.md)

**Signature:**
```solidity
function isWhitelistedToken(address token) external view returns (bool);
```

### setMaxApprovalAmount(uint256)

- **Signature**: `setMaxApprovalAmount(uint256)`
- **Visibility**: external
- **Source Range**: 3752:336:12
- **Details**: [function_setMaxApprovalAmount_uint256.md](./function_setMaxApprovalAmount_uint256.md)

**Signature:**
```solidity
function setMaxApprovalAmount(uint256 _maxApprovalAmount) external onlyAdmin();
```

### buy(bytes,contract IERC20,uint256,contract TokenHolder,address,address,address)

- **Signature**: `buy(bytes,contract IERC20,uint256,contract TokenHolder,address,address,address)`
- **Visibility**: external
- **Source Range**: 4094:2611:12
- **Details**: [function_buy_bytes_contract_IERC20_uint256_contract_TokenHolder_address_address_address.md](./function_buy_bytes_contract_IERC20_uint256_contract_TokenHolder_address_address_address.md)

**Signature:**
```solidity
function buy(bytes calldata buyingCode, IERC20 tokenCollateral, uint256 borrowAmount, TokenHolder tokenHolder, address inchRouter, address integratorFeeAddress, address whitelistedDex) external payable nonReentrant();
```

### sell(uint256,bytes,contract TokenHolder,address,address,address)

- **Signature**: `sell(uint256,bytes,contract TokenHolder,address,address,address)`
- **Visibility**: external
- **Source Range**: 6711:2619:12
- **Details**: [function_sell_uint256_bytes_contract_TokenHolder_address_address_address.md](./function_sell_uint256_bytes_contract_TokenHolder_address_address_address.md)

**Signature:**
```solidity
function sell(uint256 loanId, bytes calldata sellingCode, TokenHolder tokenHolder, address inchRouter, address integratorFeeAddress, address whitelistedDex) external payable nonReentrant();
```

### liquidate(uint256,contract TokenHolder,uint256)

- **Signature**: `liquidate(uint256,contract TokenHolder,uint256)`
- **Visibility**: external
- **Source Range**: 9336:1396:12
- **Details**: [function_liquidate_uint256_contract_TokenHolder_uint256.md](./function_liquidate_uint256_contract_TokenHolder_uint256.md)

**Signature:**
```solidity
function liquidate(uint256 loanId, TokenHolder tokenHolder, uint256 closingPositionSize) external onlyAdmin() nonReentrant();
```
