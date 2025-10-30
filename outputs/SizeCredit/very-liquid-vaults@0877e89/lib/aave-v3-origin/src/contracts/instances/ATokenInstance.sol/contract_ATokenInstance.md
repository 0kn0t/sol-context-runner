# Contract: ATokenInstance

## Metadata

- **Name**: ATokenInstance
- **Type**: Contract
- **Path**: lib/aave-v3-origin/src/contracts/instances/ATokenInstance.sol

## Implements Interfaces

- **IAToken** [lib/aave-v3-origin/src/contracts/interfaces/IAToken.sol/interface_IAToken.md]
- **IInitializableAToken** [lib/aave-v3-origin/src/contracts/interfaces/IInitializableAToken.sol/interface_IInitializableAToken.md]
- **IScaledBalanceToken** [lib/aave-v3-origin/src/contracts/interfaces/IScaledBalanceToken.sol/interface_IScaledBalanceToken.md]
- **IERC20Detailed** [lib/aave-v3-origin/src/contracts/dependencies/openzeppelin/contracts/IERC20Detailed.sol/interface_IERC20Detailed.md]
- **IERC20** [lib/aave-v3-origin/src/contracts/dependencies/openzeppelin/contracts/IERC20.sol/interface_IERC20.md]

## State Variables

### lastInitializedRevision (inherited from VersionedInitializable)

```solidity
///  @dev Indicates that the contract has been initialized.
uint256 private lastInitializedRevision = 0
```

### initializing (inherited from VersionedInitializable)

```solidity
///  @dev Indicates that the contract is in the process of being initialized.
bool private initializing
```

### ______gap (inherited from VersionedInitializable)

```solidity
uint256[50] private ______gap
```

### _userState (inherited from IncentivizedERC20)

```solidity
mapping(address => UserState) internal _userState
```

### _allowances (inherited from IncentivizedERC20)

```solidity
mapping(address => mapping(address => uint256)) private _allowances
```

### _totalSupply (inherited from IncentivizedERC20)

```solidity
uint256 internal _totalSupply
```

### _name (inherited from IncentivizedERC20)

```solidity
string private _name
```

### _symbol (inherited from IncentivizedERC20)

```solidity
string private _symbol
```

### _decimals (inherited from IncentivizedERC20)

```solidity
uint8 private _decimals
```

### _incentivesController (inherited from IncentivizedERC20)

```solidity
IAaveIncentivesController internal _incentivesController
```

**IAaveIncentivesController**: [lib/aave-v3-origin/src/contracts/interfaces/IAaveIncentivesController.sol/interface_IAaveIncentivesController.md]

### _addressesProvider (inherited from IncentivizedERC20)

```solidity
IPoolAddressesProvider internal immutable _addressesProvider
```

**IPoolAddressesProvider**: [lib/aave-v3-origin/src/contracts/interfaces/IPoolAddressesProvider.sol/interface_IPoolAddressesProvider.md]

### POOL (inherited from IncentivizedERC20)

```solidity
IPool public immutable POOL
```

**IPool**: [lib/aave-v3-origin/src/contracts/interfaces/IPool.sol/interface_IPool.md]

### EIP712_REVISION (inherited from EIP712Base)

```solidity
bytes public constant EIP712_REVISION = bytes("1")
```

### EIP712_DOMAIN (inherited from EIP712Base)

```solidity
bytes32 internal constant EIP712_DOMAIN = keccak256("EIP712Domain(string name,string version,uint256 chainId,address verifyingContract)")
```

### _nonces (inherited from EIP712Base)

```solidity
mapping(address => uint256) internal _nonces
```

### _domainSeparator (inherited from EIP712Base)

```solidity
bytes32 internal _domainSeparator
```

### _chainId (inherited from EIP712Base)

```solidity
uint256 internal immutable _chainId
```

### PERMIT_TYPEHASH (inherited from AToken)

```solidity
bytes32 public constant PERMIT_TYPEHASH = keccak256("Permit(address owner,address spender,uint256 value,uint256 nonce,uint256 deadline)")
```

### _treasury (inherited from AToken)

```solidity
address internal _treasury
```

### _underlyingAsset (inherited from AToken)

```solidity
address internal _underlyingAsset
```

### ATOKEN_REVISION

```solidity
uint256 public constant ATOKEN_REVISION = 1
```

## Structs

### UserState (inherited from IncentivizedERC20)

```solidity
///  @dev UserState - additionalData is a flexible field.
///  ATokens and VariableDebtTokens use this field store the index of the
///  user's last supply/withdrawal/borrow/repayment.
struct UserState {
    uint128 balance;
    uint128 additionalData;
}
```

## Events

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

### Initialized (inherited from IInitializableAToken)

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

### BalanceTransfer (inherited from IAToken)

```solidity
///  @dev Emitted during the transfer action
///  @param from The user whose tokens are being transferred
///  @param to The recipient
///  @param value The scaled amount being transferred
///  @param index The next liquidity index of the reserve
event BalanceTransfer(address indexed from, address indexed to, uint256 value, uint256 index);
```

## Public/External Functions

### constructor(contract IPool)

- **Signature**: `constructor(contract IPool)`
- **Visibility**: public
- **Source Range**: 297:39:10
- **Details**: [function_constructor_contract_IPool.md](./function_constructor_contract_IPool.md)

**Signature:**
```solidity
constructor(IPool pool) AToken(pool);
```

### initialize(contract IPool,address,address,contract IAaveIncentivesController,uint8,string,string,bytes)

- **Signature**: `initialize(contract IPool,address,address,contract IAaveIncentivesController,uint8,string,string,bytes)`
- **Visibility**: public
- **Source Range**: 529:850:10
- **Details**: [function_initialize_contract_IPool_address_address_contract_IAaveIncentivesController_uint8_string_string_bytes.md](./function_initialize_contract_IPool_address_address_contract_IAaveIncentivesController_uint8_string_string_bytes.md)

**Signature:**
```solidity
/// @inheritdoc IInitializableAToken
function initialize(IPool initializingPool, address treasury, address underlyingAsset, IAaveIncentivesController incentivesController, uint8 aTokenDecimals, string calldata aTokenName, string calldata aTokenSymbol, bytes calldata params) virtual override public initializer();
```

### name() (inherited from IncentivizedERC20)

- **Signature**: `name()`
- **Visibility**: public
- **Source Range**: 2864:84:35
- **Details**: [function_name.md](./function_name.md)

**Signature:**
```solidity
/// @inheritdoc IERC20Detailed
function name() override public view returns (string memory);
```

### symbol() (inherited from IncentivizedERC20)

- **Signature**: `symbol()`
- **Visibility**: external
- **Source Range**: 2985:90:35
- **Details**: [function_symbol.md](./function_symbol.md)

**Signature:**
```solidity
/// @inheritdoc IERC20Detailed
function symbol() override external view returns (string memory);
```

### decimals() (inherited from IncentivizedERC20)

- **Signature**: `decimals()`
- **Visibility**: external
- **Source Range**: 3112:86:35
- **Details**: [function_decimals.md](./function_decimals.md)

**Signature:**
```solidity
/// @inheritdoc IERC20Detailed
function decimals() override external view returns (uint8);
```

### totalSupply() (inherited from IncentivizedERC20)

- **Signature**: `totalSupply()`
- **Visibility**: public
- **Source Range**: 3227:100:35
- **Details**: [function_totalSupply.md](./function_totalSupply.md)

**Signature:**
```solidity
/// @inheritdoc IERC20
function totalSupply() virtual override public view returns (uint256);
```

### balanceOf(address) (inherited from IncentivizedERC20)

- **Signature**: `balanceOf(address)`
- **Visibility**: public
- **Source Range**: 3356:128:35
- **Details**: [function_balanceOf_address.md](./function_balanceOf_address.md)

**Signature:**
```solidity
/// @inheritdoc IERC20
function balanceOf(address account) virtual override public view returns (uint256);
```

### getIncentivesController() (inherited from IncentivizedERC20)

- **Signature**: `getIncentivesController()`
- **Visibility**: external
- **Source Range**: 3625:132:35
- **Details**: [function_getIncentivesController.md](./function_getIncentivesController.md)

**Signature:**
```solidity
///  @notice Returns the address of the Incentives Controller contract
///  @return The address of the Incentives Controller
function getIncentivesController() virtual external view returns (IAaveIncentivesController);
```

### setIncentivesController(contract IAaveIncentivesController) (inherited from IncentivizedERC20)

- **Signature**: `setIncentivesController(contract IAaveIncentivesController)`
- **Visibility**: external
- **Source Range**: 3872:139:35
- **Details**: [function_setIncentivesController_contract_IAaveIncentivesController.md](./function_setIncentivesController_contract_IAaveIncentivesController.md)

**Signature:**
```solidity
///  @notice Sets a new Incentives Controller
///  @param controller the new Incentives controller
function setIncentivesController(IAaveIncentivesController controller) external onlyPoolAdmin();
```

### transfer(address,uint256) (inherited from IncentivizedERC20)

- **Signature**: `transfer(address,uint256)`
- **Visibility**: external
- **Source Range**: 4040:213:35
- **Details**: [function_transfer_address_uint256.md](./function_transfer_address_uint256.md)

**Signature:**
```solidity
/// @inheritdoc IERC20
function transfer(address recipient, uint256 amount) virtual override external returns (bool);
```

### allowance(address,address) (inherited from IncentivizedERC20)

- **Signature**: `allowance(address,address)`
- **Visibility**: external
- **Source Range**: 4282:157:35
- **Details**: [function_allowance_address_address.md](./function_allowance_address_address.md)

**Signature:**
```solidity
/// @inheritdoc IERC20
function allowance(address owner, address spender) virtual override external view returns (uint256);
```

### approve(address,uint256) (inherited from IncentivizedERC20)

- **Signature**: `approve(address,uint256)`
- **Visibility**: external
- **Source Range**: 4468:158:35
- **Details**: [function_approve_address_uint256.md](./function_approve_address_uint256.md)

**Signature:**
```solidity
/// @inheritdoc IERC20
function approve(address spender, uint256 amount) virtual override external returns (bool);
```

### transferFrom(address,address,uint256) (inherited from IncentivizedERC20)

- **Signature**: `transferFrom(address,address,uint256)`
- **Visibility**: external
- **Source Range**: 4655:327:35
- **Details**: [function_transferFrom_address_address_uint256.md](./function_transferFrom_address_address_uint256.md)

**Signature:**
```solidity
/// @inheritdoc IERC20
function transferFrom(address sender, address recipient, uint256 amount) virtual override external returns (bool);
```

### increaseAllowance(address,uint256) (inherited from IncentivizedERC20)

- **Signature**: `increaseAllowance(address,uint256)`
- **Visibility**: external
- **Source Range**: 5230:204:35
- **Details**: [function_increaseAllowance_address_uint256.md](./function_increaseAllowance_address_uint256.md)

**Signature:**
```solidity
///  @notice Increases the allowance of spender to spend _msgSender() tokens
///  @param spender The user allowed to spend on behalf of _msgSender()
///  @param addedValue The amount being added to the allowance
///  @return `true`
function increaseAllowance(address spender, uint256 addedValue) virtual external returns (bool);
```

### decreaseAllowance(address,uint256) (inherited from IncentivizedERC20)

- **Signature**: `decreaseAllowance(address,uint256)`
- **Visibility**: external
- **Source Range**: 5692:226:35
- **Details**: [function_decreaseAllowance_address_uint256.md](./function_decreaseAllowance_address_uint256.md)

**Signature:**
```solidity
///  @notice Decreases the allowance of spender to spend _msgSender() tokens
///  @param spender The user allowed to spend on behalf of _msgSender()
///  @param subtractedValue The amount being subtracted to the allowance
///  @return `true`
function decreaseAllowance(address spender, uint256 subtractedValue) virtual external returns (bool);
```

### scaledBalanceOf(address) (inherited from ScaledBalanceTokenBase)

- **Signature**: `scaledBalanceOf(address)`
- **Visibility**: external
- **Source Range**: 1220:119:37
- **Details**: [function_scaledBalanceOf_address.md](./function_scaledBalanceOf_address.md)

**Signature:**
```solidity
/// @inheritdoc IScaledBalanceToken
function scaledBalanceOf(address user) override external view returns (uint256);
```

### getScaledUserBalanceAndSupply(address) (inherited from ScaledBalanceTokenBase)

- **Signature**: `getScaledUserBalanceAndSupply(address)`
- **Visibility**: external
- **Source Range**: 1381:173:37
- **Details**: [function_getScaledUserBalanceAndSupply_address.md](./function_getScaledUserBalanceAndSupply_address.md)

**Signature:**
```solidity
/// @inheritdoc IScaledBalanceToken
function getScaledUserBalanceAndSupply(address user) override external view returns (uint256, uint256);
```

### scaledTotalSupply() (inherited from ScaledBalanceTokenBase)

- **Signature**: `scaledTotalSupply()`
- **Visibility**: public
- **Source Range**: 1596:113:37
- **Details**: [function_scaledTotalSupply.md](./function_scaledTotalSupply.md)

**Signature:**
```solidity
/// @inheritdoc IScaledBalanceToken
function scaledTotalSupply() virtual override public view returns (uint256);
```

### getPreviousIndex(address) (inherited from ScaledBalanceTokenBase)

- **Signature**: `getPreviousIndex(address)`
- **Visibility**: external
- **Source Range**: 1751:138:37
- **Details**: [function_getPreviousIndex_address.md](./function_getPreviousIndex_address.md)

**Signature:**
```solidity
/// @inheritdoc IScaledBalanceToken
function getPreviousIndex(address user) virtual override external view returns (uint256);
```

### DOMAIN_SEPARATOR() (inherited from EIP712Base)

- **Signature**: `DOMAIN_SEPARATOR()`
- **Visibility**: public
- **Source Range**: 862:185:34
- **Details**: [function_DOMAIN_SEPARATOR.md](./function_DOMAIN_SEPARATOR.md)

**Signature:**
```solidity
///  @notice Get the domain separator for the token
///  @dev Return cached value if chainId matches cache, otherwise recomputes separator
///  @return The domain separator of the token at current chain
function DOMAIN_SEPARATOR() virtual public view returns (bytes32);
```

### nonces(address) (inherited from EIP712Base)

- **Signature**: `nonces(address)`
- **Visibility**: public
- **Source Range**: 1255:101:34
- **Details**: [function_nonces_address.md](./function_nonces_address.md)

**Signature:**
```solidity
///  @notice Returns the nonce value for address specified as parameter
///  @param owner The address for which the nonce is being returned
///  @return The nonce value for the input address`
function nonces(address owner) virtual public view returns (uint256);
```

### mint(address,address,uint256,uint256) (inherited from AToken)

- **Signature**: `mint(address,address,uint256,uint256)`
- **Visibility**: external
- **Source Range**: 2116:215:31
- **Details**: [function_mint_address_address_uint256_uint256.md](./function_mint_address_address_uint256_uint256.md)

**Signature:**
```solidity
/// @inheritdoc IAToken
function mint(address caller, address onBehalfOf, uint256 amount, uint256 index) virtual override external onlyPool() returns (bool);
```

### burn(address,address,uint256,uint256) (inherited from AToken)

- **Signature**: `burn(address,address,uint256,uint256)`
- **Visibility**: external
- **Source Range**: 2361:339:31
- **Details**: [function_burn_address_address_uint256_uint256.md](./function_burn_address_address_uint256_uint256.md)

**Signature:**
```solidity
/// @inheritdoc IAToken
function burn(address from, address receiverOfUnderlying, uint256 amount, uint256 index) virtual override external onlyPool();
```

### mintToTreasury(uint256,uint256) (inherited from AToken)

- **Signature**: `mintToTreasury(uint256,uint256)`
- **Visibility**: external
- **Source Range**: 2730:196:31
- **Details**: [function_mintToTreasury_uint256_uint256.md](./function_mintToTreasury_uint256_uint256.md)

**Signature:**
```solidity
/// @inheritdoc IAToken
function mintToTreasury(uint256 amount, uint256 index) virtual override external onlyPool();
```

### transferOnLiquidation(address,address,uint256) (inherited from AToken)

- **Signature**: `transferOnLiquidation(address,address,uint256)`
- **Visibility**: external
- **Source Range**: 2956:296:31
- **Details**: [function_transferOnLiquidation_address_address_uint256.md](./function_transferOnLiquidation_address_address_uint256.md)

**Signature:**
```solidity
/// @inheritdoc IAToken
function transferOnLiquidation(address from, address to, uint256 value) virtual override external onlyPool();
```

### RESERVE_TREASURY_ADDRESS() (inherited from AToken)

- **Signature**: `RESERVE_TREASURY_ADDRESS()`
- **Visibility**: external
- **Source Range**: 3859:104:31
- **Details**: [function_RESERVE_TREASURY_ADDRESS.md](./function_RESERVE_TREASURY_ADDRESS.md)

**Signature:**
```solidity
/// @inheritdoc IAToken
function RESERVE_TREASURY_ADDRESS() override external view returns (address);
```

### UNDERLYING_ASSET_ADDRESS() (inherited from AToken)

- **Signature**: `UNDERLYING_ASSET_ADDRESS()`
- **Visibility**: external
- **Source Range**: 3993:111:31
- **Details**: [function_UNDERLYING_ASSET_ADDRESS.md](./function_UNDERLYING_ASSET_ADDRESS.md)

**Signature:**
```solidity
/// @inheritdoc IAToken
function UNDERLYING_ASSET_ADDRESS() override external view returns (address);
```

### transferUnderlyingTo(address,uint256) (inherited from AToken)

- **Signature**: `transferUnderlyingTo(address,uint256)`
- **Visibility**: external
- **Source Range**: 4134:161:31
- **Details**: [function_transferUnderlyingTo_address_uint256.md](./function_transferUnderlyingTo_address_uint256.md)

**Signature:**
```solidity
/// @inheritdoc IAToken
function transferUnderlyingTo(address target, uint256 amount) virtual override external onlyPool();
```

### handleRepayment(address,address,uint256) (inherited from AToken)

- **Signature**: `handleRepayment(address,address,uint256)`
- **Visibility**: external
- **Source Range**: 4325:163:31
- **Details**: [function_handleRepayment_address_address_uint256.md](./function_handleRepayment_address_address_uint256.md)

**Signature:**
```solidity
/// @inheritdoc IAToken
function handleRepayment(address user, address onBehalfOf, uint256 amount) virtual override external onlyPool();
```

### permit(address,address,uint256,uint256,uint8,bytes32,bytes32) (inherited from AToken)

- **Signature**: `permit(address,address,uint256,uint256,uint8,bytes32,bytes32)`
- **Visibility**: external
- **Source Range**: 4518:755:31
- **Details**: [function_permit_address_address_uint256_uint256_uint8_bytes32_bytes32.md](./function_permit_address_address_uint256_uint256_uint8_bytes32_bytes32.md)

**Signature:**
```solidity
/// @inheritdoc IAToken
function permit(address owner, address spender, uint256 value, uint256 deadline, uint8 v, bytes32 r, bytes32 s) override external;
```

### rescueTokens(address,address,uint256) (inherited from AToken)

- **Signature**: `rescueTokens(address,address,uint256)`
- **Visibility**: external
- **Source Range**: 7315:223:31
- **Details**: [function_rescueTokens_address_address_uint256.md](./function_rescueTokens_address_address_uint256.md)

**Signature:**
```solidity
/// @inheritdoc IAToken
function rescueTokens(address token, address to, uint256 amount) override external onlyPoolAdmin();
```
