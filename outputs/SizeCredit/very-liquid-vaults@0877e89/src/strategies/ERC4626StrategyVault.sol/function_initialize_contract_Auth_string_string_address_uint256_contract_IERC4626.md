# Function: initialize(contract Auth,string,string,address,uint256,contract IERC4626)

**Contract**: [src/strategies/ERC4626StrategyVault.sol/contract_ERC4626StrategyVault.md]

## Metadata

- **Contract**: ERC4626StrategyVault
- **Signature**: `initialize(contract Auth,string,string,address,uint256,contract IERC4626)`
- **Visibility**: public
- **Source Range**: 1870:568:148

## Implementation

```solidity
/// @notice Initializes the ERC4626StrategyVault with an external vault
///  @dev Sets the external vault and calls parent initialization
function initialize(Auth auth_, string memory name_, string memory symbol_, address fundingAccount, uint256 firstDepositAmount, IERC4626 vault_) virtual public initializer() {
    if (address(vault_) == address(0)) revert NullAddress();
    ERC4626StrategyVaultStorage storage $ = _getERC4626StrategyVaultStorage();
    $._vault = vault_;
    emit VaultSet(address(vault_));
    super.initialize(auth_, IERC20(address(vault_.asset())), name_, symbol_, fundingAccount, firstDepositAmount);
}
```

## Related Implementations

### _getERC4626StrategyVaultStorage()

- **Kind**: internal
- **Source**: 1444:198:148
- **Link**: `src/strategies/ERC4626StrategyVault.sol:ERC4626StrategyVault:_getERC4626StrategyVaultStorage()`

```solidity
function _getERC4626StrategyVaultStorage() private pure returns (ERC4626StrategyVaultStorage storage $) {
    assembly {
        $.slot := ERC4626StrategyVaultStorageLocation
    }
}
```

### initialize(contract Auth,contract IERC20,string,string,address,uint256)

- **Kind**: internal
- **Source**: 3788:820:149
- **Link**: `src/utils/BaseVault.sol:BaseVault:initialize(contract Auth,contract IERC20,string,string,address,uint256)`

```solidity
/// @notice Initializes the BaseVault with necessary parameters
///  @param auth_ The address of the Auth contract
///  @param asset_ The address of the asset
///  @param name_ The name of the vault
///  @param symbol_ The symbol of the vault
///  @param fundingAccount_ The address of the funding account for the first deposit, which will be treated as dead shares
///  @param firstDepositAmount_ The amount of the first deposit, which will be treated as dead shares
///  @dev Sets up all inherited contracts and makes the first deposit to prevent inflation attacks
function initialize(Auth auth_, IERC20 asset_, string memory name_, string memory symbol_, address fundingAccount_, uint256 firstDepositAmount_) virtual public initializer() {
    __ERC4626_init(asset_);
    __ERC20_init(name_, symbol_);
    __ERC20Permit_init(name_);
    __ReentrancyGuard_init();
    __Pausable_init();
    __Multicall_init();
    __UUPSUpgradeable_init();
    if (address(auth_) == address(0)) revert NullAddress();
    if (firstDepositAmount_ == 0) revert NullAmount();
    BaseVaultStorage storage $ = _getBaseVaultStorage();
    $._auth = auth_;
    emit AuthSet(address(auth_));
    _setTotalAssetsCap(type(uint256).max);
    _firstDeposit(fundingAccount_, firstDepositAmount_);
}
```

### __ERC4626_init(contract IERC20)

- **Kind**: internal
- **Source**: 5095:114:60
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/token/ERC20/extensions/ERC4626Upgradeable.sol:ERC4626Upgradeable:__ERC4626_init(contract IERC20)`

```solidity
///  @dev Set the underlying asset contract. This must be an ERC20-compatible contract (ERC-20 or ERC-777).
function __ERC4626_init(IERC20 asset_) internal onlyInitializing() {
    __ERC4626_init_unchained(asset_);
}
```

### __ERC4626_init_unchained(contract IERC20)

- **Kind**: internal
- **Source**: 5215:304:60
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/token/ERC20/extensions/ERC4626Upgradeable.sol:ERC4626Upgradeable:__ERC4626_init_unchained(contract IERC20)`

```solidity
function __ERC4626_init_unchained(IERC20 asset_) internal onlyInitializing() {
    ERC4626Storage storage $ = _getERC4626Storage();
    (bool success, uint8 assetDecimals) = _tryGetAssetDecimals(asset_);
    $._underlyingDecimals = success ? assetDecimals : 18;
    $._asset = asset_;
}
```

### _getERC4626Storage()

- **Kind**: internal
- **Source**: 4088:159:60
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/token/ERC20/extensions/ERC4626Upgradeable.sol:ERC4626Upgradeable:_getERC4626Storage()`

```solidity
function _getERC4626Storage() private pure returns (ERC4626Storage storage $) {
    assembly {
        $.slot := ERC4626StorageLocation
    }
}
```

### _tryGetAssetDecimals(contract IERC20)

- **Kind**: internal
- **Source**: 5662:550:60
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/token/ERC20/extensions/ERC4626Upgradeable.sol:ERC4626Upgradeable:_tryGetAssetDecimals(contract IERC20)`

```solidity
///  @dev Attempts to fetch the asset decimals. A return value of false indicates that the attempt failed in some way.
function _tryGetAssetDecimals(IERC20 asset_) private view returns (bool ok, uint8 assetDecimals) {
    (bool success, bytes memory encodedDecimals) = address(asset_).staticcall(abi.encodeCall(IERC20Metadata.decimals, ()));
    if (success && (encodedDecimals.length >= 32)) {
        uint256 returnedDecimals = abi.decode(encodedDecimals, (uint256));
        if (returnedDecimals <= type(uint8).max) {
            return (true, uint8(returnedDecimals));
        }
    }
    return (false, 0);
}
```

### onlyInitializing()

- **Kind**: modifier
- **Source**: 6891:76:56
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/proxy/utils/Initializable.sol:Initializable:onlyInitializing()`

```solidity
///  @dev Modifier to protect an initialization function so that it can only be invoked by functions with the
///  {initializer} and {reinitializer} modifiers, directly or indirectly.
modifier onlyInitializing() {
    _checkInitializing();
    _;
}
```

### _checkInitializing()

- **Kind**: internal
- **Source**: 7082:141:56
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/proxy/utils/Initializable.sol:Initializable:_checkInitializing()`

```solidity
///  @dev Reverts if the contract is not in an initializing state. See {onlyInitializing}.
function _checkInitializing() virtual internal view {
    if (!_isInitializing()) {
        revert NotInitializing();
    }
}
```

### _isInitializing()

- **Kind**: internal
- **Source**: 8485:120:56
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/proxy/utils/Initializable.sol:Initializable:_isInitializing()`

```solidity
///  @dev Returns `true` if the contract is currently initializing. See {onlyInitializing}.
function _isInitializing() internal view returns (bool) {
    return _getInitializableStorage()._initializing;
}
```

### _getInitializableStorage()

- **Kind**: internal
- **Source**: 9071:205:56
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/proxy/utils/Initializable.sol:Initializable:_getInitializableStorage()`

```solidity
///  @dev Returns a pointer to the storage namespace.
function _getInitializableStorage() private pure returns (InitializableStorage storage $) {
    bytes32 slot = _initializableStorageSlot();
    assembly {
        $.slot := slot
    }
}
```

### _initializableStorageSlot()

- **Kind**: internal
- **Source**: 8819:122:56
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/proxy/utils/Initializable.sol:Initializable:_initializableStorageSlot()`

```solidity
///  @dev Pointer to storage slot. Allows integrators to override it with a custom storage location.
///  NOTE: Consider following the ERC-7201 formula to derive storage locations.
function _initializableStorageSlot() virtual internal pure returns (bytes32) {
    return INITIALIZABLE_STORAGE;
}
```

### __ERC20_init(string,string)

- **Kind**: internal
- **Source**: 2263:147:58
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/token/ERC20/ERC20Upgradeable.sol:ERC20Upgradeable:__ERC20_init(string,string)`

```solidity
///  @dev Sets the values for {name} and {symbol}.
///  Both values are immutable: they can only be set once during construction.
function __ERC20_init(string memory name_, string memory symbol_) internal onlyInitializing() {
    __ERC20_init_unchained(name_, symbol_);
}
```

### __ERC20_init_unchained(string,string)

- **Kind**: internal
- **Source**: 2416:216:58
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/token/ERC20/ERC20Upgradeable.sol:ERC20Upgradeable:__ERC20_init_unchained(string,string)`

```solidity
function __ERC20_init_unchained(string memory name_, string memory symbol_) internal onlyInitializing() {
    ERC20Storage storage $ = _getERC20Storage();
    $._name = name_;
    $._symbol = symbol_;
}
```

### _getERC20Storage()

- **Kind**: internal
- **Source**: 1947:153:58
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/token/ERC20/ERC20Upgradeable.sol:ERC20Upgradeable:_getERC20Storage()`

```solidity
function _getERC20Storage() private pure returns (ERC20Storage storage $) {
    assembly {
        $.slot := ERC20StorageLocation
    }
}
```

### __ERC20Permit_init(string)

- **Kind**: internal
- **Source**: 1832:125:59
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/token/ERC20/extensions/ERC20PermitUpgradeable.sol:ERC20PermitUpgradeable:__ERC20Permit_init(string)`

```solidity
///  @dev Initializes the {EIP712} domain separator using the `name` parameter, and setting `version` to `"1"`.
///  It's a good idea to use the same `name` that is defined as the ERC-20 token name.
function __ERC20Permit_init(string memory name) internal onlyInitializing() {
    __EIP712_init_unchained(name, "1");
}
```

### __EIP712_init_unchained(string,string)

- **Kind**: internal
- **Source**: 3599:330:66
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/utils/cryptography/EIP712Upgradeable.sol:EIP712Upgradeable:__EIP712_init_unchained(string,string)`

```solidity
function __EIP712_init_unchained(string memory name, string memory version) internal onlyInitializing() {
    EIP712Storage storage $ = _getEIP712Storage();
    $._name = name;
    $._version = version;
    $._hashedName = 0;
    $._hashedVersion = 0;
}
```

### _getEIP712Storage()

- **Kind**: internal
- **Source**: 2720:156:66
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/utils/cryptography/EIP712Upgradeable.sol:EIP712Upgradeable:_getEIP712Storage()`

```solidity
function _getEIP712Storage() private pure returns (EIP712Storage storage $) {
    assembly {
        $.slot := EIP712StorageLocation
    }
}
```

### __ReentrancyGuard_init()

- **Kind**: internal
- **Source**: 2684:111:65
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/utils/ReentrancyGuardUpgradeable.sol:ReentrancyGuardUpgradeable:__ReentrancyGuard_init()`

```solidity
function __ReentrancyGuard_init() internal onlyInitializing() {
    __ReentrancyGuard_init_unchained();
}
```

### __ReentrancyGuard_init_unchained()

- **Kind**: internal
- **Source**: 2801:183:65
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/utils/ReentrancyGuardUpgradeable.sol:ReentrancyGuardUpgradeable:__ReentrancyGuard_init_unchained()`

```solidity
function __ReentrancyGuard_init_unchained() internal onlyInitializing() {
    ReentrancyGuardStorage storage $ = _getReentrancyGuardStorage();
    $._status = NOT_ENTERED;
}
```

### _getReentrancyGuardStorage()

- **Kind**: internal
- **Source**: 2395:183:65
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/utils/ReentrancyGuardUpgradeable.sol:ReentrancyGuardUpgradeable:_getReentrancyGuardStorage()`

```solidity
function _getReentrancyGuardStorage() private pure returns (ReentrancyGuardStorage storage $) {
    assembly {
        $.slot := ReentrancyGuardStorageLocation
    }
}
```

### __Pausable_init()

- **Kind**: internal
- **Source**: 2266:60:64
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/utils/PausableUpgradeable.sol:PausableUpgradeable:__Pausable_init()`

```solidity
function __Pausable_init() internal onlyInitializing() {}
```

### __Multicall_init()

- **Kind**: internal
- **Source**: 1218:61:62
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/utils/MulticallUpgradeable.sol:MulticallUpgradeable:__Multicall_init()`

```solidity
function __Multicall_init() internal onlyInitializing() {}
```

### __UUPSUpgradeable_init()

- **Kind**: internal
- **Source**: 2970:67:57
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/proxy/utils/UUPSUpgradeable.sol:UUPSUpgradeable:__UUPSUpgradeable_init()`

```solidity
function __UUPSUpgradeable_init() internal onlyInitializing() {}
```

### _getBaseVaultStorage()

- **Kind**: internal
- **Source**: 2552:165:149
- **Link**: `src/utils/BaseVault.sol:BaseVault:_getBaseVaultStorage()`

```solidity
function _getBaseVaultStorage() private pure returns (BaseVaultStorage storage $) {
    assembly {
        $.slot := BaseVaultStorageLocation
    }
}
```

### _setTotalAssetsCap(uint256)

- **Kind**: internal
- **Source**: 7238:297:149
- **Link**: `src/utils/BaseVault.sol:BaseVault:_setTotalAssetsCap(uint256)`

```solidity
/// @notice Internal function to set the total assets cap
function _setTotalAssetsCap(uint256 totalAssetsCap_) private {
    BaseVaultStorage storage $ = _getBaseVaultStorage();
    uint256 oldTotalAssetsCap = $._totalAssetsCap;
    $._totalAssetsCap = totalAssetsCap_;
    emit TotalAssetsCapSet(oldTotalAssetsCap, totalAssetsCap_);
}
```

### _firstDeposit(address,uint256)

- **Kind**: internal
- **Source**: 7758:442:149
- **Link**: `src/utils/BaseVault.sol:BaseVault:_firstDeposit(address,uint256)`

```solidity
/// @notice This function is used to deposit the first amount of assets into the vault
///  @dev This is equivalent to deposit(firstDepositAmount_, address(this)); with _msgSender() replaced by fundingAccount_
function _firstDeposit(address fundingAccount_, uint256 firstDepositAmount_) private {
    address receiver = address(this);
    uint256 maxAssets = maxDeposit(receiver);
    if (firstDepositAmount_ > maxAssets) revert ERC4626ExceededMaxDeposit(receiver, firstDepositAmount_, maxAssets);
    uint256 shares = previewDeposit(firstDepositAmount_);
    _deposit(fundingAccount_, receiver, firstDepositAmount_, shares);
}
```

### maxDeposit(address)

- **Kind**: internal
- **Source**: 2534:183:148
- **Link**: `src/strategies/ERC4626StrategyVault.sol:ERC4626StrategyVault:maxDeposit(address)`

```solidity
/// @notice Returns the maximum amount that can be deposited
function maxDeposit(address receiver) override(BaseVault) public view returns (uint256) {
    return Math.min(vault().maxDeposit(address(this)), super.maxDeposit(receiver));
}
```

### min(uint256,uint256)

- **Kind**: internal
- **Source**: 5617:111:109
- **Link**: `lib/openzeppelin-contracts/contracts/utils/math/Math.sol:Math:min(uint256,uint256)`

```solidity
///  @dev Returns the smallest of two numbers.
function min(uint256 a, uint256 b) internal pure returns (uint256) {
    return ternary(a < b, a, b);
}
```

### vault()

- **Kind**: internal
- **Source**: 5486:112:148
- **Link**: `src/strategies/ERC4626StrategyVault.sol:ERC4626StrategyVault:vault()`

```solidity
/// @notice Returns the external vault
function vault() public view returns (IERC4626) {
    return _getERC4626StrategyVaultStorage()._vault;
}
```

### maxDeposit(address)

- **Kind**: internal
- **Source**: 12364:318:149
- **Link**: `src/utils/BaseVault.sol:BaseVault:maxDeposit(address)`

```solidity
/// @inheritdoc ERC4626Upgradeable
function maxDeposit(address receiver) virtual override(ERC4626Upgradeable, IERC4626) public view returns (uint256) {
    return _pausedOrAuthPaused() ? 0 : ((_totalAssetsCap() == type(uint256).max) ? super.maxDeposit(receiver) : _maxDeposit());
}
```

### _pausedOrAuthPaused()

- **Kind**: internal
- **Source**: 8334:110:149
- **Link**: `src/utils/BaseVault.sol:BaseVault:_pausedOrAuthPaused()`

```solidity
/// @notice Returns true if the vault is paused
///  @dev Checks both local pause state and global pause state from Auth
function _pausedOrAuthPaused() private view returns (bool) {
    return paused() || auth().paused();
}
```

### auth()

- **Kind**: internal
- **Source**: 14480:104:149
- **Link**: `src/utils/BaseVault.sol:BaseVault:auth()`

```solidity
/// @inheritdoc IVault
function auth() override public view returns (Auth) {
    return _getBaseVaultStorage()._auth;
}
```

### paused()

- **Kind**: internal
- **Source**: 2496:145:64
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/utils/PausableUpgradeable.sol:PausableUpgradeable:paused()`

```solidity
///  @dev Returns true if the contract is paused, and false otherwise.
function paused() virtual public view returns (bool) {
    PausableStorage storage $ = _getPausableStorage();
    return $._paused;
}
```

### _getPausableStorage()

- **Kind**: internal
- **Source**: 1147:162:64
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/utils/PausableUpgradeable.sol:PausableUpgradeable:_getPausableStorage()`

```solidity
function _getPausableStorage() private pure returns (PausableStorage storage $) {
    assembly {
        $.slot := PausableStorageLocation
    }
}
```

### _totalAssetsCap()

- **Kind**: internal
- **Source**: 14811:120:149
- **Link**: `src/utils/BaseVault.sol:BaseVault:_totalAssetsCap()`

```solidity
/// @notice Internal function to return the total assets cap
function _totalAssetsCap() private view returns (uint256) {
    return _getBaseVaultStorage()._totalAssetsCap;
}
```

### maxDeposit(address)

- **Kind**: internal
- **Source**: 7663:108:60
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/token/ERC20/extensions/ERC4626Upgradeable.sol:ERC4626Upgradeable:maxDeposit(address)`

```solidity
/// @dev See {IERC4626-maxDeposit}. 
function maxDeposit(address) virtual public view returns (uint256) {
    return type(uint256).max;
}
```

### _maxDeposit()

- **Kind**: internal
- **Source**: 14295:130:149
- **Link**: `src/utils/BaseVault.sol:BaseVault:_maxDeposit()`

```solidity
/// @notice Internal function to calculate the maximum amount that can be deposited
///  @dev The maximum amount that can be deposited is the total assets cap minus the total assets
function _maxDeposit() private view returns (uint256) {
    return Math.saturatingSub(_totalAssetsCap(), totalAssets());
}
```

### saturatingSub(uint256,uint256)

- **Kind**: internal
- **Source**: 4217:150:109
- **Link**: `lib/openzeppelin-contracts/contracts/utils/math/Math.sol:Math:saturatingSub(uint256,uint256)`

```solidity
///  @dev Unsigned saturating subtraction, bounds to zero instead of overflowing.
function saturatingSub(uint256 a, uint256 b) internal pure returns (uint256) {
    (, uint256 result) = trySub(a, b);
    return result;
}
```

### totalAssets()

- **Kind**: internal
- **Source**: 4255:181:148
- **Link**: `src/strategies/ERC4626StrategyVault.sol:ERC4626StrategyVault:totalAssets()`

```solidity
/// @notice Returns the total assets managed by this strategy
///  @dev Converts the external vault shares held by this contract to asset value
///  @return The total assets under management
function totalAssets() virtual override(ERC4626Upgradeable, IERC4626) public view returns (uint256) {
    return vault().convertToAssets(vault().balanceOf(address(this)));
}
```

### trySub(uint256,uint256)

- **Kind**: internal
- **Source**: 2052:240:109
- **Link**: `lib/openzeppelin-contracts/contracts/utils/math/Math.sol:Math:trySub(uint256,uint256)`

```solidity
///  @dev Returns the subtraction of two unsigned integers, with a success flag (no overflow).
function trySub(uint256 a, uint256 b) internal pure returns (bool success, uint256 result) {
    unchecked {
        uint256 c = a - b;
        success = c <= a;
        result = c * SafeCast.toUint(success);
    }
}
```

### toUint(bool)

- **Kind**: internal
- **Source**: 34795:145:110
- **Link**: `lib/openzeppelin-contracts/contracts/utils/math/SafeCast.sol:SafeCast:toUint(bool)`

```solidity
///  @dev Cast a boolean (false or true) to a uint256 (0 or 1) with no jump.
function toUint(bool b) internal pure returns (uint256 u) {
    assembly ("memory-safe") {
        u := iszero(iszero(b))
    }
}
```

### ternary(bool,uint256,uint256)

- **Kind**: internal
- **Source**: 5071:294:109
- **Link**: `lib/openzeppelin-contracts/contracts/utils/math/Math.sol:Math:ternary(bool,uint256,uint256)`

```solidity
///  @dev Branchless ternary evaluation for `a ? b : c`. Gas costs are constant.
///  IMPORTANT: This function may reduce bytecode size and consume less gas when used standalone.
///  However, the compiler may optimize Solidity ternary operations (i.e. `a ? b : c`) to only compute
///  one branch when needed, making this function more expensive.
function ternary(bool condition, uint256 a, uint256 b) internal pure returns (uint256) {
    unchecked {
        return b ^ ((a ^ b) * SafeCast.toUint(condition));
    }
}
```

### previewDeposit(uint256)

- **Kind**: internal
- **Source**: 8338:147:60
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/token/ERC20/extensions/ERC4626Upgradeable.sol:ERC4626Upgradeable:previewDeposit(uint256)`

```solidity
/// @dev See {IERC4626-previewDeposit}. 
function previewDeposit(uint256 assets) virtual public view returns (uint256) {
    return _convertToShares(assets, Math.Rounding.Floor);
}
```

### _convertToShares(uint256,enum Math.Rounding)

- **Kind**: internal
- **Source**: 10972:213:60
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/token/ERC20/extensions/ERC4626Upgradeable.sol:ERC4626Upgradeable:_convertToShares(uint256,enum Math.Rounding)`

```solidity
///  @dev Internal conversion function (from assets to shares) with support for rounding direction.
function _convertToShares(uint256 assets, Math.Rounding rounding) virtual internal view returns (uint256) {
    return assets.mulDiv(totalSupply() + (10 ** _decimalsOffset()), totalAssets() + 1, rounding);
}
```

### mulDiv(uint256,uint256,uint256,enum Math.Rounding)

- **Kind**: internal
- **Source**: 11054:238:109
- **Link**: `lib/openzeppelin-contracts/contracts/utils/math/Math.sol:Math:mulDiv(uint256,uint256,uint256,enum Math.Rounding)`

```solidity
///  @dev Calculates x * y / denominator with full precision, following the selected rounding direction.
function mulDiv(uint256 x, uint256 y, uint256 denominator, Rounding rounding) internal pure returns (uint256) {
    return mulDiv(x, y, denominator) + SafeCast.toUint(unsignedRoundsUp(rounding) && (mulmod(x, y, denominator) > 0));
}
```

### _decimalsOffset()

- **Kind**: internal
- **Source**: 13425:90:60
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/token/ERC20/extensions/ERC4626Upgradeable.sol:ERC4626Upgradeable:_decimalsOffset()`

```solidity
function _decimalsOffset() virtual internal view returns (uint8) {
    return 0;
}
```

### totalSupply()

- **Kind**: internal
- **Source**: 3877:152:58
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/token/ERC20/ERC20Upgradeable.sol:ERC20Upgradeable:totalSupply()`

```solidity
///  @dev See {IERC20-totalSupply}.
function totalSupply() virtual public view returns (uint256) {
    ERC20Storage storage $ = _getERC20Storage();
    return $._totalSupply;
}
```

### unsignedRoundsUp(enum Math.Rounding)

- **Kind**: internal
- **Source**: 32020:122:109
- **Link**: `lib/openzeppelin-contracts/contracts/utils/math/Math.sol:Math:unsignedRoundsUp(enum Math.Rounding)`

```solidity
///  @dev Returns whether a provided rounding mode is considered rounding up for unsigned integers.
function unsignedRoundsUp(Rounding rounding) internal pure returns (bool) {
    return (uint8(rounding) % 2) == 1;
}
```

### mulDiv(uint256,uint256,uint256)

- **Kind**: internal
- **Source**: 7242:3683:109
- **Link**: `lib/openzeppelin-contracts/contracts/utils/math/Math.sol:Math:mulDiv(uint256,uint256,uint256)`

```solidity
///  @dev Calculates floor(x * y / denominator) with full precision. Throws if result overflows a uint256 or
///  denominator == 0.
///  Original credit to Remco Bloemen under MIT license (https://xn--2-umb.com/21/muldiv) with further edits by
///  Uniswap Labs also under MIT license.
function mulDiv(uint256 x, uint256 y, uint256 denominator) internal pure returns (uint256 result) {
    unchecked {
        (uint256 high, uint256 low) = mul512(x, y);
        if (high == 0) {
            return low / denominator;
        }
        if (denominator <= high) {
            Panic.panic(ternary(denominator == 0, Panic.DIVISION_BY_ZERO, Panic.UNDER_OVERFLOW));
        }
        uint256 remainder;
        assembly ("memory-safe") {
            remainder := mulmod(x, y, denominator)
            high := sub(high, gt(remainder, low))
            low := sub(low, remainder)
        }
        uint256 twos = denominator & (0 - denominator);
        assembly ("memory-safe") {
            denominator := div(denominator, twos)
            low := div(low, twos)
            twos := add(div(sub(0, twos), twos), 1)
        }
        low |= high * twos;
        uint256 inverse = (3 * denominator) ^ 2;
        inverse *= 2 - (denominator * inverse);
        inverse *= 2 - (denominator * inverse);
        inverse *= 2 - (denominator * inverse);
        inverse *= 2 - (denominator * inverse);
        inverse *= 2 - (denominator * inverse);
        inverse *= 2 - (denominator * inverse);
        result = low * inverse;
        return result;
    }
}
```

### mul512(uint256,uint256)

- **Kind**: internal
- **Source**: 1027:550:109
- **Link**: `lib/openzeppelin-contracts/contracts/utils/math/Math.sol:Math:mul512(uint256,uint256)`

```solidity
///  @dev Return the 512-bit multiplication of two uint256.
///  The result is stored in two 256 variables such that product = high * 2²⁵⁶ + low.
function mul512(uint256 a, uint256 b) internal pure returns (uint256 high, uint256 low) {
    assembly ("memory-safe") {
        let mm := mulmod(a, b, not(0))
        low := mul(a, b)
        high := sub(sub(mm, low), lt(mm, low))
    }
}
```

### panic(uint256)

- **Kind**: internal
- **Source**: 1776:194:101
- **Link**: `lib/openzeppelin-contracts/contracts/utils/Panic.sol:Panic:panic(uint256)`

```solidity
/// @dev Reverts with a panic code. Recommended to use with
///  the internal constants with predefined codes.
function panic(uint256 code) internal pure {
    assembly ("memory-safe") {
        mstore(0x00, 0x4e487b71)
        mstore(0x20, code)
        revert(0x1c, 0x24)
    }
}
```

### _deposit(address,address,uint256,uint256)

- **Kind**: internal
- **Source**: 4599:330:148
- **Link**: `src/strategies/ERC4626StrategyVault.sol:ERC4626StrategyVault:_deposit(address,address,uint256,uint256)`

```solidity
/// @notice Internal deposit function that invests in the external vault
///  @dev Calls parent deposit then invests the assets in the external vault
function _deposit(address caller, address receiver, uint256 assets, uint256 shares) override internal {
    super._deposit(caller, receiver, assets, shares);
    IERC20(asset()).forceApprove(address(vault()), assets);
    vault().deposit(assets, address(this));
}
```

### _deposit(address,address,uint256,uint256)

- **Kind**: internal
- **Source**: 11570:291:149
- **Link**: `src/utils/BaseVault.sol:BaseVault:_deposit(address,address,uint256,uint256)`

```solidity
/// @notice Deposits assets into the vault
///  @inheritdoc ERC4626Upgradeable
///  @dev Prevents deposits that would result in 0 shares received
function _deposit(address caller, address receiver, uint256 assets, uint256 shares) virtual override internal {
    if ((assets > 0) && (shares == 0)) revert NullAmount();
    super._deposit(caller, receiver, assets, shares);
}
```

### _deposit(address,address,uint256,uint256)

- **Kind**: internal
- **Source**: 11586:841:60
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/token/ERC20/extensions/ERC4626Upgradeable.sol:ERC4626Upgradeable:_deposit(address,address,uint256,uint256)`

```solidity
///  @dev Deposit/mint common workflow.
function _deposit(address caller, address receiver, uint256 assets, uint256 shares) virtual internal {
    SafeERC20.safeTransferFrom(IERC20(asset()), caller, address(this), assets);
    _mint(receiver, shares);
    emit Deposit(caller, receiver, assets, shares);
}
```

### safeTransferFrom(contract IERC20,address,address,uint256)

- **Kind**: internal
- **Source**: 1618:188:92
- **Link**: `lib/openzeppelin-contracts/contracts/token/ERC20/utils/SafeERC20.sol:SafeERC20:safeTransferFrom(contract IERC20,address,address,uint256)`

```solidity
///  @dev Transfer `value` amount of `token` from `from` to `to`, spending the approval given by `from` to the
///  calling contract. If `token` returns no value, non-reverting calls are assumed to be successful.
function safeTransferFrom(IERC20 token, address from, address to, uint256 value) internal {
    _callOptionalReturn(token, abi.encodeCall(token.transferFrom, (from, to, value)));
}
```

### asset()

- **Kind**: internal
- **Source**: 6882:153:60
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/token/ERC20/extensions/ERC4626Upgradeable.sol:ERC4626Upgradeable:asset()`

```solidity
/// @dev See {IERC4626-asset}. 
function asset() virtual public view returns (address) {
    ERC4626Storage storage $ = _getERC4626Storage();
    return address($._asset);
}
```

### _callOptionalReturn(contract IERC20,bytes)

- **Kind**: internal
- **Source**: 8370:720:92
- **Link**: `lib/openzeppelin-contracts/contracts/token/ERC20/utils/SafeERC20.sol:SafeERC20:_callOptionalReturn(contract IERC20,bytes)`

```solidity
///  @dev Imitates a Solidity high-level call (i.e. a regular function call to a contract), relaxing the requirement
///  on the return value: the return value is optional (but if data is returned, it must not be false).
///  @param token The token targeted by the call.
///  @param data The call data (encoded using abi.encode or one of its variants).
///  This is a variant of {_callOptionalReturnBool} that reverts if call fails to meet the requirements.
function _callOptionalReturn(IERC20 token, bytes memory data) private {
    uint256 returnSize;
    uint256 returnValue;
    assembly ("memory-safe") {
        let success := call(gas(), token, 0, add(data, 0x20), mload(data), 0, 0x20)
        if iszero(success) {
            let ptr := mload(0x40)
            returndatacopy(ptr, 0, returndatasize())
            revert(ptr, returndatasize())
        }
        returnSize := returndatasize()
        returnValue := mload(0)
    }
    if ((returnSize == 0) ? (address(token).code.length == 0) : (returnValue != 1)) {
        revert SafeERC20FailedOperation(address(token));
    }
}
```

### _mint(address,uint256)

- **Kind**: internal
- **Source**: 8714:208:58
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/token/ERC20/ERC20Upgradeable.sol:ERC20Upgradeable:_mint(address,uint256)`

```solidity
///  @dev Creates a `value` amount of tokens and assigns them to `account`, by transferring it from address(0).
///  Relies on the `_update` mechanism
///  Emits a {Transfer} event with `from` set to the zero address.
///  NOTE: This function is not virtual, {_update} should be overridden instead.
function _mint(address account, uint256 value) internal {
    if (account == address(0)) {
        revert ERC20InvalidReceiver(address(0));
    }
    _update(address(0), account, value);
}
```

### _update(address,address,uint256)

- **Kind**: internal
- **Source**: 7201:1170:58
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/token/ERC20/ERC20Upgradeable.sol:ERC20Upgradeable:_update(address,address,uint256)`

```solidity
///  @dev Transfers a `value` amount of tokens from `from` to `to`, or alternatively mints (or burns) if `from`
///  (or `to`) is the zero address. All customizations to transfers, mints, and burns should be done by overriding
///  this function.
///  Emits a {Transfer} event.
function _update(address from, address to, uint256 value) virtual internal {
    ERC20Storage storage $ = _getERC20Storage();
    if (from == address(0)) {
        $._totalSupply += value;
    } else {
        uint256 fromBalance = $._balances[from];
        if (fromBalance < value) {
            revert ERC20InsufficientBalance(from, fromBalance, value);
        }
        unchecked {
            $._balances[from] = fromBalance - value;
        }
    }
    if (to == address(0)) {
        unchecked {
            $._totalSupply -= value;
        }
    } else {
        unchecked {
            $._balances[to] += value;
        }
    }
    emit Transfer(from, to, value);
}
```

### initializer()

- **Kind**: modifier
- **Source**: 4069:1102:56
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/proxy/utils/Initializable.sol:Initializable:initializer()`

```solidity
///  @dev A modifier that defines a protected initializer function that can be invoked at most once. In its scope,
///  `onlyInitializing` functions can be used to initialize parent contracts.
///  Similar to `reinitializer(1)`, except that in the context of a constructor an `initializer` may be invoked any
///  number of times. This behavior in the constructor can be useful during testing and is not expected to be used in
///  production.
///  Emits an {Initialized} event.
modifier initializer() {
    InitializableStorage storage $ = _getInitializableStorage();
    bool isTopLevelCall = !$._initializing;
    uint64 initialized = $._initialized;
    bool initialSetup = (initialized == 0) && isTopLevelCall;
    bool construction = (initialized == 1) && (address(this).code.length == 0);
    if ((!initialSetup) && (!construction)) {
        revert InvalidInitialization();
    }
    $._initialized = 1;
    if (isTopLevelCall) {
        $._initializing = true;
    }
    _;
    if (isTopLevelCall) {
        $._initializing = false;
        emit Initialized(1);
    }
}
```

## External Calls

- **IERC4626::asset()**

## State Variable Reads

- **INITIALIZABLE_STORAGE** (`bytes32`)
- **NOT_ENTERED** (`uint256`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: ERC4626StrategyVault.initialize(contract Auth,string,string,address,uint256,contract IERC4626) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
  ├─ [1] ⚙️ FUNCTION: ERC4626StrategyVault._getERC4626StrategyVaultStorage() (NodeID: 1)
  │   💬 Args: [no args]
  │   👁️  Def: private
  ├─ [1] ⚙️ FUNCTION: BaseVault.initialize(contract Auth,contract IERC20,string,string,address,uint256) (NodeID: 2)
  │   💬 Args: [auth_, IERC20(address(vault_.asset())), name_, symbol_, fundingAccount, firstDepositAmount]
  │   👁️  Def: public
  │ ├─ [2] ⚙️ FUNCTION: ERC4626Upgradeable.__ERC4626_init(contract IERC20) (NodeID: 3)
  │ │   💬 Args: [asset_]
  │ │   👁️  Def: internal
  │ │ ├─ [3] ⚙️ FUNCTION: ERC4626Upgradeable.__ERC4626_init_unchained(contract IERC20) (NodeID: 4)
  │ │ │   💬 Args: [asset_]
  │ │ │   👁️  Def: internal
  │ │ │ ├─ [4] ⚙️ FUNCTION: ERC4626Upgradeable._getERC4626Storage() (NodeID: 5)
  │ │ │ │   💬 Args: [no args]
  │ │ │ │   👁️  Def: private
  │ │ │ ├─ [4] ⚙️ FUNCTION: ERC4626Upgradeable._tryGetAssetDecimals(contract IERC20) (NodeID: 6)
  │ │ │ │   💬 Args: [asset_]
  │ │ │ │   👁️  Def: private
  │ │ │ └─ [4] 🔒 MODIFIER: Initializable.onlyInitializing() (NodeID: 7)
  │ │ │     💬 Args: [no args]
  │ │ │   └─ [5] ⚙️ FUNCTION: Initializable._checkInitializing() (NodeID: 8)
  │ │ │       💬 Args: [no args]
  │ │ │       👁️  Def: internal
  │ │ │     └─ [6] ⚙️ FUNCTION: Initializable._isInitializing() (NodeID: 9)
  │ │ │         💬 Args: [no args]
  │ │ │         👁️  Def: internal
  │ │ │       └─ [7] ⚙️ FUNCTION: Initializable._getInitializableStorage() (NodeID: 10)
  │ │ │           💬 Args: [no args]
  │ │ │           👁️  Def: private
  │ │ │         └─ [8] ⚙️ FUNCTION: Initializable._initializableStorageSlot() (NodeID: 11)
  │ │ │             💬 Args: [no args]
  │ │ │             👁️  Def: internal
  │ │ └─ [3] 🔒 MODIFIER: Initializable.onlyInitializing() (NodeID: 12)
  │ │     💬 Args: [no args]
  │ │   └─ [4] ⚙️ FUNCTION: Initializable._checkInitializing() (NodeID: 13)
  │ │       💬 Args: [no args]
  │ │       👁️  Def: internal
  │ │     └─ [5] ⚙️ FUNCTION: Initializable._isInitializing() (NodeID: 14)
  │ │         💬 Args: [no args]
  │ │         👁️  Def: internal
  │ │       └─ [6] ⚙️ FUNCTION: Initializable._getInitializableStorage() (NodeID: 15)
  │ │           💬 Args: [no args]
  │ │           👁️  Def: private
  │ │         └─ [7] ⚙️ FUNCTION: Initializable._initializableStorageSlot() (NodeID: 16)
  │ │             💬 Args: [no args]
  │ │             👁️  Def: internal
  │ ├─ [2] ⚙️ FUNCTION: ERC20Upgradeable.__ERC20_init(string,string) (NodeID: 17)
  │ │   💬 Args: [name_, symbol_]
  │ │   👁️  Def: internal
  │ │ ├─ [3] ⚙️ FUNCTION: ERC20Upgradeable.__ERC20_init_unchained(string,string) (NodeID: 18)
  │ │ │   💬 Args: [name_, symbol_]
  │ │ │   👁️  Def: internal
  │ │ │ ├─ [4] ⚙️ FUNCTION: ERC20Upgradeable._getERC20Storage() (NodeID: 19)
  │ │ │ │   💬 Args: [no args]
  │ │ │ │   👁️  Def: private
  │ │ │ └─ [4] 🔒 MODIFIER: Initializable.onlyInitializing() (NodeID: 20)
  │ │ │     💬 Args: [no args]
  │ │ │   └─ [5] ⚙️ FUNCTION: Initializable._checkInitializing() (NodeID: 21)
  │ │ │       💬 Args: [no args]
  │ │ │       👁️  Def: internal
  │ │ │     └─ [6] ⚙️ FUNCTION: Initializable._isInitializing() (NodeID: 22)
  │ │ │         💬 Args: [no args]
  │ │ │         👁️  Def: internal
  │ │ │       └─ [7] ⚙️ FUNCTION: Initializable._getInitializableStorage() (NodeID: 23)
  │ │ │           💬 Args: [no args]
  │ │ │           👁️  Def: private
  │ │ │         └─ [8] ⚙️ FUNCTION: Initializable._initializableStorageSlot() (NodeID: 24)
  │ │ │             💬 Args: [no args]
  │ │ │             👁️  Def: internal
  │ │ └─ [3] 🔒 MODIFIER: Initializable.onlyInitializing() (NodeID: 25)
  │ │     💬 Args: [no args]
  │ │   └─ [4] ⚙️ FUNCTION: Initializable._checkInitializing() (NodeID: 26)
  │ │       💬 Args: [no args]
  │ │       👁️  Def: internal
  │ │     └─ [5] ⚙️ FUNCTION: Initializable._isInitializing() (NodeID: 27)
  │ │         💬 Args: [no args]
  │ │         👁️  Def: internal
  │ │       └─ [6] ⚙️ FUNCTION: Initializable._getInitializableStorage() (NodeID: 28)
  │ │           💬 Args: [no args]
  │ │           👁️  Def: private
  │ │         └─ [7] ⚙️ FUNCTION: Initializable._initializableStorageSlot() (NodeID: 29)
  │ │             💬 Args: [no args]
  │ │             👁️  Def: internal
  │ ├─ [2] ⚙️ FUNCTION: ERC20PermitUpgradeable.__ERC20Permit_init(string) (NodeID: 30)
  │ │   💬 Args: [name_]
  │ │   👁️  Def: internal
  │ │ ├─ [3] ⚙️ FUNCTION: EIP712Upgradeable.__EIP712_init_unchained(string,string) (NodeID: 31)
  │ │ │   💬 Args: [name, "1"]
  │ │ │   👁️  Def: internal
  │ │ │ ├─ [4] ⚙️ FUNCTION: EIP712Upgradeable._getEIP712Storage() (NodeID: 32)
  │ │ │ │   💬 Args: [no args]
  │ │ │ │   👁️  Def: private
  │ │ │ └─ [4] 🔒 MODIFIER: Initializable.onlyInitializing() (NodeID: 33)
  │ │ │     💬 Args: [no args]
  │ │ │   └─ [5] ⚙️ FUNCTION: Initializable._checkInitializing() (NodeID: 34)
  │ │ │       💬 Args: [no args]
  │ │ │       👁️  Def: internal
  │ │ │     └─ [6] ⚙️ FUNCTION: Initializable._isInitializing() (NodeID: 35)
  │ │ │         💬 Args: [no args]
  │ │ │         👁️  Def: internal
  │ │ │       └─ [7] ⚙️ FUNCTION: Initializable._getInitializableStorage() (NodeID: 36)
  │ │ │           💬 Args: [no args]
  │ │ │           👁️  Def: private
  │ │ │         └─ [8] ⚙️ FUNCTION: Initializable._initializableStorageSlot() (NodeID: 37)
  │ │ │             💬 Args: [no args]
  │ │ │             👁️  Def: internal
  │ │ └─ [3] 🔒 MODIFIER: Initializable.onlyInitializing() (NodeID: 38)
  │ │     💬 Args: [no args]
  │ │   └─ [4] ⚙️ FUNCTION: Initializable._checkInitializing() (NodeID: 39)
  │ │       💬 Args: [no args]
  │ │       👁️  Def: internal
  │ │     └─ [5] ⚙️ FUNCTION: Initializable._isInitializing() (NodeID: 40)
  │ │         💬 Args: [no args]
  │ │         👁️  Def: internal
  │ │       └─ [6] ⚙️ FUNCTION: Initializable._getInitializableStorage() (NodeID: 41)
  │ │           💬 Args: [no args]
  │ │           👁️  Def: private
  │ │         └─ [7] ⚙️ FUNCTION: Initializable._initializableStorageSlot() (NodeID: 42)
  │ │             💬 Args: [no args]
  │ │             👁️  Def: internal
  │ ├─ [2] ⚙️ FUNCTION: ReentrancyGuardUpgradeable.__ReentrancyGuard_init() (NodeID: 43)
  │ │   💬 Args: [no args]
  │ │   👁️  Def: internal
  │ │ ├─ [3] ⚙️ FUNCTION: ReentrancyGuardUpgradeable.__ReentrancyGuard_init_unchained() (NodeID: 44)
  │ │ │   💬 Args: [no args]
  │ │ │   👁️  Def: internal
  │ │ │ ├─ [4] ⚙️ FUNCTION: ReentrancyGuardUpgradeable._getReentrancyGuardStorage() (NodeID: 45)
  │ │ │ │   💬 Args: [no args]
  │ │ │ │   👁️  Def: private
  │ │ │ └─ [4] 🔒 MODIFIER: Initializable.onlyInitializing() (NodeID: 46)
  │ │ │     💬 Args: [no args]
  │ │ │   └─ [5] ⚙️ FUNCTION: Initializable._checkInitializing() (NodeID: 47)
  │ │ │       💬 Args: [no args]
  │ │ │       👁️  Def: internal
  │ │ │     └─ [6] ⚙️ FUNCTION: Initializable._isInitializing() (NodeID: 48)
  │ │ │         💬 Args: [no args]
  │ │ │         👁️  Def: internal
  │ │ │       └─ [7] ⚙️ FUNCTION: Initializable._getInitializableStorage() (NodeID: 49)
  │ │ │           💬 Args: [no args]
  │ │ │           👁️  Def: private
  │ │ │         └─ [8] ⚙️ FUNCTION: Initializable._initializableStorageSlot() (NodeID: 50)
  │ │ │             💬 Args: [no args]
  │ │ │             👁️  Def: internal
  │ │ └─ [3] 🔒 MODIFIER: Initializable.onlyInitializing() (NodeID: 51)
  │ │     💬 Args: [no args]
  │ │   └─ [4] ⚙️ FUNCTION: Initializable._checkInitializing() (NodeID: 52)
  │ │       💬 Args: [no args]
  │ │       👁️  Def: internal
  │ │     └─ [5] ⚙️ FUNCTION: Initializable._isInitializing() (NodeID: 53)
  │ │         💬 Args: [no args]
  │ │         👁️  Def: internal
  │ │       └─ [6] ⚙️ FUNCTION: Initializable._getInitializableStorage() (NodeID: 54)
  │ │           💬 Args: [no args]
  │ │           👁️  Def: private
  │ │         └─ [7] ⚙️ FUNCTION: Initializable._initializableStorageSlot() (NodeID: 55)
  │ │             💬 Args: [no args]
  │ │             👁️  Def: internal
  │ ├─ [2] ⚙️ FUNCTION: PausableUpgradeable.__Pausable_init() (NodeID: 56)
  │ │   💬 Args: [no args]
  │ │   👁️  Def: internal
  │ │ └─ [3] 🔒 MODIFIER: Initializable.onlyInitializing() (NodeID: 57)
  │ │     💬 Args: [no args]
  │ │   └─ [4] ⚙️ FUNCTION: Initializable._checkInitializing() (NodeID: 58)
  │ │       💬 Args: [no args]
  │ │       👁️  Def: internal
  │ │     └─ [5] ⚙️ FUNCTION: Initializable._isInitializing() (NodeID: 59)
  │ │         💬 Args: [no args]
  │ │         👁️  Def: internal
  │ │       └─ [6] ⚙️ FUNCTION: Initializable._getInitializableStorage() (NodeID: 60)
  │ │           💬 Args: [no args]
  │ │           👁️  Def: private
  │ │         └─ [7] ⚙️ FUNCTION: Initializable._initializableStorageSlot() (NodeID: 61)
  │ │             💬 Args: [no args]
  │ │             👁️  Def: internal
  │ ├─ [2] ⚙️ FUNCTION: MulticallUpgradeable.__Multicall_init() (NodeID: 62)
  │ │   💬 Args: [no args]
  │ │   👁️  Def: internal
  │ │ └─ [3] 🔒 MODIFIER: Initializable.onlyInitializing() (NodeID: 63)
  │ │     💬 Args: [no args]
  │ │   └─ [4] ⚙️ FUNCTION: Initializable._checkInitializing() (NodeID: 64)
  │ │       💬 Args: [no args]
  │ │       👁️  Def: internal
  │ │     └─ [5] ⚙️ FUNCTION: Initializable._isInitializing() (NodeID: 65)
  │ │         💬 Args: [no args]
  │ │         👁️  Def: internal
  │ │       └─ [6] ⚙️ FUNCTION: Initializable._getInitializableStorage() (NodeID: 66)
  │ │           💬 Args: [no args]
  │ │           👁️  Def: private
  │ │         └─ [7] ⚙️ FUNCTION: Initializable._initializableStorageSlot() (NodeID: 67)
  │ │             💬 Args: [no args]
  │ │             👁️  Def: internal
  │ ├─ [2] ⚙️ FUNCTION: UUPSUpgradeable.__UUPSUpgradeable_init() (NodeID: 68)
  │ │   💬 Args: [no args]
  │ │   👁️  Def: internal
  │ │ └─ [3] 🔒 MODIFIER: Initializable.onlyInitializing() (NodeID: 69)
  │ │     💬 Args: [no args]
  │ │   └─ [4] ⚙️ FUNCTION: Initializable._checkInitializing() (NodeID: 70)
  │ │       💬 Args: [no args]
  │ │       👁️  Def: internal
  │ │     └─ [5] ⚙️ FUNCTION: Initializable._isInitializing() (NodeID: 71)
  │ │         💬 Args: [no args]
  │ │         👁️  Def: internal
  │ │       └─ [6] ⚙️ FUNCTION: Initializable._getInitializableStorage() (NodeID: 72)
  │ │           💬 Args: [no args]
  │ │           👁️  Def: private
  │ │         └─ [7] ⚙️ FUNCTION: Initializable._initializableStorageSlot() (NodeID: 73)
  │ │             💬 Args: [no args]
  │ │             👁️  Def: internal
  │ ├─ [2] ⚙️ FUNCTION: BaseVault._getBaseVaultStorage() (NodeID: 74)
  │ │   💬 Args: [no args]
  │ │   👁️  Def: private
  │ ├─ [2] ⚙️ FUNCTION: BaseVault._setTotalAssetsCap(uint256) (NodeID: 75)
  │ │   💬 Args: [type(uint256).max]
  │ │   👁️  Def: private
  │ │ └─ [3] ⚙️ FUNCTION: BaseVault._getBaseVaultStorage() (NodeID: 76)
  │ │     💬 Args: [no args]
  │ │     👁️  Def: private
  │ ├─ [2] ⚙️ FUNCTION: BaseVault._firstDeposit(address,uint256) (NodeID: 77)
  │ │   💬 Args: [fundingAccount_, firstDepositAmount_]
  │ │   👁️  Def: private
  │ │ ├─ [3] ⚙️ FUNCTION: ERC4626StrategyVault.maxDeposit(address) (NodeID: 78)
  │ │ │   💬 Args: [receiver]
  │ │ │   👁️  Def: public
  │ │ │ └─ [4] ⚙️ FUNCTION: Math.min(uint256,uint256) (NodeID: 79)
  │ │ │     💬 Args: [vault().maxDeposit(address(this)), super.maxDeposit(receiver)]
  │ │ │     👁️  Def: internal
  │ │ │   ├─ [5] ⚙️ FUNCTION: ERC4626StrategyVault.vault() (NodeID: 82)
  │ │ │   │   💬 Args: [no args]
  │ │ │   │   👁️  Def: public
  │ │ │   │ └─ [6] ⚙️ FUNCTION: ERC4626StrategyVault._getERC4626StrategyVaultStorage() (NodeID: 83)
  │ │ │   │     💬 Args: [no args]
  │ │ │   │     👁️  Def: private
  │ │ │   ├─ [5] ⚙️ FUNCTION: BaseVault.maxDeposit(address) (NodeID: 84)
  │ │ │   │   💬 Args: [receiver]
  │ │ │   │   👁️  Def: public
  │ │ │   │ ├─ [6] ⚙️ FUNCTION: BaseVault._pausedOrAuthPaused() (NodeID: 85)
  │ │ │   │ │   💬 Args: [no args]
  │ │ │   │ │   👁️  Def: private
  │ │ │   │ │ ├─ [7] ⚙️ FUNCTION: BaseVault.auth() (NodeID: 86)
  │ │ │   │ │ │   💬 Args: [no args]
  │ │ │   │ │ │   👁️  Def: public
  │ │ │   │ │ │ └─ [8] ⚙️ FUNCTION: BaseVault._getBaseVaultStorage() (NodeID: 87)
  │ │ │   │ │ │     💬 Args: [no args]
  │ │ │   │ │ │     👁️  Def: private
  │ │ │   │ │ └─ [7] ⚙️ FUNCTION: PausableUpgradeable.paused() (NodeID: 88)
  │ │ │   │ │     💬 Args: [no args]
  │ │ │   │ │     👁️  Def: public
  │ │ │   │ │   └─ [8] ⚙️ FUNCTION: PausableUpgradeable._getPausableStorage() (NodeID: 89)
  │ │ │   │ │       💬 Args: [no args]
  │ │ │   │ │       👁️  Def: private
  │ │ │   │ ├─ [6] ⚙️ FUNCTION: BaseVault._totalAssetsCap() (NodeID: 90)
  │ │ │   │ │   💬 Args: [no args]
  │ │ │   │ │   👁️  Def: private
  │ │ │   │ │ └─ [7] ⚙️ FUNCTION: BaseVault._getBaseVaultStorage() (NodeID: 91)
  │ │ │   │ │     💬 Args: [no args]
  │ │ │   │ │     👁️  Def: private
  │ │ │   │ ├─ [6] ⚙️ FUNCTION: ERC4626Upgradeable.maxDeposit(address) (NodeID: 92)
  │ │ │   │ │   💬 Args: [receiver]
  │ │ │   │ │   👁️  Def: public
  │ │ │   │ └─ [6] ⚙️ FUNCTION: BaseVault._maxDeposit() (NodeID: 93)
  │ │ │   │     💬 Args: [no args]
  │ │ │   │     👁️  Def: private
  │ │ │   │   └─ [7] ⚙️ FUNCTION: Math.saturatingSub(uint256,uint256) (NodeID: 94)
  │ │ │   │       💬 Args: [_totalAssetsCap(), totalAssets()]
  │ │ │   │       👁️  Def: internal
  │ │ │   │     ├─ [8] ⚙️ FUNCTION: BaseVault._totalAssetsCap() (NodeID: 97)
  │ │ │   │     │   💬 Args: [no args]
  │ │ │   │     │   👁️  Def: private
  │ │ │   │     │ └─ [9] ⚙️ FUNCTION: BaseVault._getBaseVaultStorage() (NodeID: 98)
  │ │ │   │     │     💬 Args: [no args]
  │ │ │   │     │     👁️  Def: private
  │ │ │   │     ├─ [8] ⚙️ FUNCTION: ERC4626StrategyVault.totalAssets() (NodeID: 99)
  │ │ │   │     │   💬 Args: [no args]
  │ │ │   │     │   👁️  Def: public
  │ │ │   │     │ ├─ [9] ⚙️ FUNCTION: ERC4626StrategyVault.vault() (NodeID: 100)
  │ │ │   │     │ │   💬 Args: [no args]
  │ │ │   │     │ │   👁️  Def: public
  │ │ │   │     │ │ └─ [10] ⚙️ FUNCTION: ERC4626StrategyVault._getERC4626StrategyVaultStorage() (NodeID: 101)
  │ │ │   │     │ │     💬 Args: [no args]
  │ │ │   │     │ │     👁️  Def: private
  │ │ │   │     │ └─ [9] ⚙️ FUNCTION: ERC4626StrategyVault.vault() (NodeID: 102)
  │ │ │   │     │     💬 Args: [no args]
  │ │ │   │     │     👁️  Def: public
  │ │ │   │     │   └─ [10] ⚙️ FUNCTION: ERC4626StrategyVault._getERC4626StrategyVaultStorage() (NodeID: 103)
  │ │ │   │     │       💬 Args: [no args]
  │ │ │   │     │       👁️  Def: private
  │ │ │   │     └─ [8] ⚙️ FUNCTION: Math.trySub(uint256,uint256) (NodeID: 95)
  │ │ │   │         💬 Args: [a, b]
  │ │ │   │         👁️  Def: internal
  │ │ │   │       └─ [9] ⚙️ FUNCTION: SafeCast.toUint(bool) (NodeID: 96)
  │ │ │   │           💬 Args: [success]
  │ │ │   │           👁️  Def: internal
  │ │ │   └─ [5] ⚙️ FUNCTION: Math.ternary(bool,uint256,uint256) (NodeID: 80)
  │ │ │       💬 Args: [a < b, a, b]
  │ │ │       👁️  Def: internal
  │ │ │     └─ [6] ⚙️ FUNCTION: SafeCast.toUint(bool) (NodeID: 81)
  │ │ │         💬 Args: [condition]
  │ │ │         👁️  Def: internal
  │ │ ├─ [3] ⚙️ FUNCTION: ERC4626Upgradeable.previewDeposit(uint256) (NodeID: 104)
  │ │ │   💬 Args: [firstDepositAmount_]
  │ │ │   👁️  Def: public
  │ │ │ └─ [4] ⚙️ FUNCTION: ERC4626Upgradeable._convertToShares(uint256,enum Math.Rounding) (NodeID: 105)
  │ │ │     💬 Args: [assets, Math.Rounding.Floor]
  │ │ │     👁️  Def: internal
  │ │ │   └─ [5] ⚙️ FUNCTION: Math.mulDiv(uint256,uint256,uint256,enum Math.Rounding) (NodeID: 106)
  │ │ │       💬 Args: [assets, totalSupply() + (10 ** _decimalsOffset()), totalAssets() + 1, rounding]
  │ │ │       👁️  Def: internal
  │ │ │     ├─ [6] ⚙️ FUNCTION: ERC4626Upgradeable._decimalsOffset() (NodeID: 114)
  │ │ │     │   💬 Args: [no args]
  │ │ │     │   👁️  Def: internal
  │ │ │     ├─ [6] ⚙️ FUNCTION: ERC20Upgradeable.totalSupply() (NodeID: 115)
  │ │ │     │   💬 Args: [no args]
  │ │ │     │   👁️  Def: public
  │ │ │     │ └─ [7] ⚙️ FUNCTION: ERC20Upgradeable._getERC20Storage() (NodeID: 116)
  │ │ │     │     💬 Args: [no args]
  │ │ │     │     👁️  Def: private
  │ │ │     ├─ [6] ⚙️ FUNCTION: ERC4626StrategyVault.totalAssets() (NodeID: 117)
  │ │ │     │   💬 Args: [no args]
  │ │ │     │   👁️  Def: public
  │ │ │     │ ├─ [7] ⚙️ FUNCTION: ERC4626StrategyVault.vault() (NodeID: 118)
  │ │ │     │ │   💬 Args: [no args]
  │ │ │     │ │   👁️  Def: public
  │ │ │     │ │ └─ [8] ⚙️ FUNCTION: ERC4626StrategyVault._getERC4626StrategyVaultStorage() (NodeID: 119)
  │ │ │     │ │     💬 Args: [no args]
  │ │ │     │ │     👁️  Def: private
  │ │ │     │ └─ [7] ⚙️ FUNCTION: ERC4626StrategyVault.vault() (NodeID: 120)
  │ │ │     │     💬 Args: [no args]
  │ │ │     │     👁️  Def: public
  │ │ │     │   └─ [8] ⚙️ FUNCTION: ERC4626StrategyVault._getERC4626StrategyVaultStorage() (NodeID: 121)
  │ │ │     │       💬 Args: [no args]
  │ │ │     │       👁️  Def: private
  │ │ │     ├─ [6] ⚙️ FUNCTION: SafeCast.toUint(bool) (NodeID: 107)
  │ │ │     │   💬 Args: [unsignedRoundsUp(rounding) && (mulmod(x, y, denominator) > 0)]
  │ │ │     │   👁️  Def: internal
  │ │ │     │ └─ [7] ⚙️ FUNCTION: Math.unsignedRoundsUp(enum Math.Rounding) (NodeID: 108)
  │ │ │     │     💬 Args: [rounding]
  │ │ │     │     👁️  Def: internal
  │ │ │     └─ [6] ⚙️ FUNCTION: Math.mulDiv(uint256,uint256,uint256) (NodeID: 109)
  │ │ │         💬 Args: [x, y, denominator]
  │ │ │         👁️  Def: internal
  │ │ │       ├─ [7] ⚙️ FUNCTION: Math.mul512(uint256,uint256) (NodeID: 110)
  │ │ │       │   💬 Args: [x, y]
  │ │ │       │   👁️  Def: internal
  │ │ │       └─ [7] ⚙️ FUNCTION: Panic.panic(uint256) (NodeID: 111)
  │ │ │           💬 Args: [ternary(denominator == 0, Panic.DIVISION_BY_ZERO, Panic.UNDER_OVERFLOW)]
  │ │ │           👁️  Def: internal
  │ │ │         └─ [8] ⚙️ FUNCTION: Math.ternary(bool,uint256,uint256) (NodeID: 112)
  │ │ │             💬 Args: [denominator == 0, Panic.DIVISION_BY_ZERO, Panic.UNDER_OVERFLOW]
  │ │ │             👁️  Def: internal
  │ │ │           └─ [9] ⚙️ FUNCTION: SafeCast.toUint(bool) (NodeID: 113)
  │ │ │               💬 Args: [condition]
  │ │ │               👁️  Def: internal
  │ │ └─ [3] ⚙️ FUNCTION: ERC4626StrategyVault._deposit(address,address,uint256,uint256) (NodeID: 122)
  │ │     💬 Args: [fundingAccount_, receiver, firstDepositAmount_, shares]
  │ │     👁️  Def: internal
  │ │   ├─ [4] ⚙️ FUNCTION: BaseVault._deposit(address,address,uint256,uint256) (NodeID: 123)
  │ │   │   💬 Args: [caller, receiver, assets, shares]
  │ │   │   👁️  Def: internal
  │ │   │ └─ [5] ⚙️ FUNCTION: ERC4626Upgradeable._deposit(address,address,uint256,uint256) (NodeID: 124)
  │ │   │     💬 Args: [caller, receiver, assets, shares]
  │ │   │     👁️  Def: internal
  │ │   │   ├─ [6] ⚙️ FUNCTION: SafeERC20.safeTransferFrom(contract IERC20,address,address,uint256) (NodeID: 125)
  │ │   │   │   💬 Args: [IERC20(asset()), caller, address(this), assets]
  │ │   │   │   👁️  Def: internal
  │ │   │   │ ├─ [7] ⚙️ FUNCTION: ERC4626Upgradeable.asset() (NodeID: 127)
  │ │   │   │ │   💬 Args: [no args]
  │ │   │   │ │   👁️  Def: public
  │ │   │   │ │ └─ [8] ⚙️ FUNCTION: ERC4626Upgradeable._getERC4626Storage() (NodeID: 128)
  │ │   │   │ │     💬 Args: [no args]
  │ │   │   │ │     👁️  Def: private
  │ │   │   │ └─ [7] ⚙️ FUNCTION: SafeERC20._callOptionalReturn(contract IERC20,bytes) (NodeID: 126)
  │ │   │   │     💬 Args: [token, abi.encodeCall(token.transferFrom, (from, to, value))]
  │ │   │   │     👁️  Def: private
  │ │   │   └─ [6] ⚙️ FUNCTION: ERC20Upgradeable._mint(address,uint256) (NodeID: 129)
  │ │   │       💬 Args: [receiver, shares]
  │ │   │       👁️  Def: internal
  │ │   │     └─ [7] ⚙️ FUNCTION: ERC20Upgradeable._update(address,address,uint256) (NodeID: 130)
  │ │   │         💬 Args: [address(0), account, value]
  │ │   │         👁️  Def: internal
  │ │   │       └─ [8] ⚙️ FUNCTION: ERC20Upgradeable._getERC20Storage() (NodeID: 131)
  │ │   │           💬 Args: [no args]
  │ │   │           👁️  Def: private
  │ │   ├─ [4] ⚙️ FUNCTION: ERC4626Upgradeable.asset() (NodeID: 132)
  │ │   │   💬 Args: [no args]
  │ │   │   👁️  Def: public
  │ │   │ └─ [5] ⚙️ FUNCTION: ERC4626Upgradeable._getERC4626Storage() (NodeID: 133)
  │ │   │     💬 Args: [no args]
  │ │   │     👁️  Def: private
  │ │   ├─ [4] ⚙️ FUNCTION: ERC4626StrategyVault.vault() (NodeID: 134)
  │ │   │   💬 Args: [no args]
  │ │   │   👁️  Def: public
  │ │   │ └─ [5] ⚙️ FUNCTION: ERC4626StrategyVault._getERC4626StrategyVaultStorage() (NodeID: 135)
  │ │   │     💬 Args: [no args]
  │ │   │     👁️  Def: private
  │ │   └─ [4] ⚙️ FUNCTION: ERC4626StrategyVault.vault() (NodeID: 136)
  │ │       💬 Args: [no args]
  │ │       👁️  Def: public
  │ │     └─ [5] ⚙️ FUNCTION: ERC4626StrategyVault._getERC4626StrategyVaultStorage() (NodeID: 137)
  │ │         💬 Args: [no args]
  │ │         👁️  Def: private
  │ └─ [2] 🔒 MODIFIER: Initializable.initializer() (NodeID: 138)
  │     💬 Args: [no args]
  │   └─ [3] ⚙️ FUNCTION: Initializable._getInitializableStorage() (NodeID: 139)
  │       💬 Args: [no args]
  │       👁️  Def: private
  │     └─ [4] ⚙️ FUNCTION: Initializable._initializableStorageSlot() (NodeID: 140)
  │         💬 Args: [no args]
  │         👁️  Def: internal
  └─ [1] 🔒 MODIFIER: Initializable.initializer() (NodeID: 141)
      💬 Args: [no args]
    └─ [2] ⚙️ FUNCTION: Initializable._getInitializableStorage() (NodeID: 142)
        💬 Args: [no args]
        👁️  Def: private
      └─ [3] ⚙️ FUNCTION: Initializable._initializableStorageSlot() (NodeID: 143)
          💬 Args: [no args]
          👁️  Def: internal
```

## Documentation

### Function Documentation

@notice Initializes the ERC4626StrategyVault with an external vault
 @dev Sets the external vault and calls parent initialization
