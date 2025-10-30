# Function: initialize(contract Auth,contract IERC20,string,string,address,uint256,contract IVault[])

**Contract**: [src/VeryLiquidVault.sol/contract_VeryLiquidVault.md]

## Metadata

- **Contract**: VeryLiquidVault
- **Signature**: `initialize(contract Auth,contract IERC20,string,string,address,uint256,contract IVault[])`
- **Visibility**: public
- **Source Range**: 4041:658:145

## Implementation

```solidity
/// @notice Initializes the VeryLiquidVault with strategies
///  @param auth_ The address of the Auth contract
///  @param asset_ The address of the asset
///  @param name_ The name of the vault
///  @param symbol_ The symbol of the vault
///  @param fundingAccount The address of the funding account for the first deposit, which will be treated as dead shares
///  @param firstDepositAmount The amount of the first deposit, which will be treated as dead shares
///  @param strategies_ The initial strategies to add to the vault
function initialize(Auth auth_, IERC20 asset_, string memory name_, string memory symbol_, address fundingAccount, uint256 firstDepositAmount, IVault[] memory strategies_) virtual public initializer() {
    __PerformanceVault_init(auth_.getRoleMember(DEFAULT_ADMIN_ROLE, 0), 0);
    for (uint256 i = 0; i < strategies_.length; ++i) {
        _addStrategy(strategies_[i], address(asset_), address(auth_));
    }
    _setRebalanceMaxSlippagePercent(DEFAULT_MAX_SLIPPAGE_PERCENT);
    super.initialize(auth_, asset_, name_, symbol_, fundingAccount, firstDepositAmount);
}
```

## Related Implementations

### __PerformanceVault_init(address,uint256)

- **Kind**: internal
- **Source**: 2153:221:151
- **Link**: `src/utils/PerformanceVault.sol:PerformanceVault:__PerformanceVault_init(address,uint256)`

```solidity
/// @notice Initializes the PerformanceVault with a fee recipient and performance fee percent
///  @param feeRecipient_ The address of the fee recipient
///  @param performanceFeePercent_ The performance fee percent
function __PerformanceVault_init(address feeRecipient_, uint256 performanceFeePercent_) internal onlyInitializing() {
    _setFeeRecipient(feeRecipient_);
    _setPerformanceFeePercent(performanceFeePercent_);
}
```

### _setFeeRecipient(address)

- **Kind**: internal
- **Source**: 3809:364:151
- **Link**: `src/utils/PerformanceVault.sol:PerformanceVault:_setFeeRecipient(address)`

```solidity
/// @notice Sets the fee recipient
///  @param feeRecipient_ The new fee recipient
function _setFeeRecipient(address feeRecipient_) internal {
    if (feeRecipient_ == address(0)) revert NullAddress();
    PerformanceVaultStorage storage $ = _getPerformanceVaultStorage();
    address feeRecipientBefore = $._feeRecipient;
    $._feeRecipient = feeRecipient_;
    emit FeeRecipientSet(feeRecipientBefore, feeRecipient_);
}
```

### _getPerformanceVaultStorage()

- **Kind**: internal
- **Source**: 1114:186:151
- **Link**: `src/utils/PerformanceVault.sol:PerformanceVault:_getPerformanceVaultStorage()`

```solidity
function _getPerformanceVaultStorage() private pure returns (PerformanceVaultStorage storage $) {
    assembly {
        $.slot := PerformanceVaultStorageLocation
    }
}
```

### _setPerformanceFeePercent(uint256)

- **Kind**: internal
- **Source**: 2826:887:151
- **Link**: `src/utils/PerformanceVault.sol:PerformanceVault:_setPerformanceFeePercent(uint256)`

```solidity
/// @notice Sets the performance fee percent
///  @param performanceFeePercent_ The new performance fee percent
///  @dev Reverts if the performance fee percent is greater than the maximum performance fee percent
function _setPerformanceFeePercent(uint256 performanceFeePercent_) internal {
    if (performanceFeePercent_ > MAXIMUM_PERFORMANCE_FEE_PERCENT) {
        revert PerformanceFeePercentTooHigh(performanceFeePercent_, MAXIMUM_PERFORMANCE_FEE_PERCENT);
    }
    PerformanceVaultStorage storage $ = _getPerformanceVaultStorage();
    uint256 performanceFeePercentBefore = $._performanceFeePercent;
    uint256 highWaterMarkBefore = $._highWaterMark;
    uint256 currentPPS = _pps();
    if (((performanceFeePercentBefore == 0) && (performanceFeePercent_ > 0)) && (highWaterMarkBefore < currentPPS)) {
        _setHighWaterMark(currentPPS);
    }
    $._performanceFeePercent = performanceFeePercent_;
    emit PerformanceFeePercentSet(performanceFeePercentBefore, performanceFeePercent_);
}
```

### _pps()

- **Kind**: internal
- **Source**: 4240:240:151
- **Link**: `src/utils/PerformanceVault.sol:PerformanceVault:_pps()`

```solidity
/// @notice Internal function to get the price per share
function _pps() private view returns (uint256) {
    uint256 totalAssets_ = totalAssets();
    uint256 totalSupply_ = totalSupply();
    return (totalSupply_ > 0) ? Math.mulDiv(totalAssets_, PERCENT, totalSupply_) : PERCENT;
}
```

### totalAssets()

- **Kind**: internal
- **Source**: 7057:583:145
- **Link**: `src/VeryLiquidVault.sol:VeryLiquidVault:totalAssets()`

```solidity
/// @inheritdoc ERC4626Upgradeable
///  @dev The total assets is the sum of the assets in all strategies
function totalAssets() virtual override(ERC4626Upgradeable, IERC4626) public view returns (uint256 total) {
    VeryLiquidVaultStorage storage $ = _getVeryLiquidVaultStorage();
    uint256 length = $._strategies.length;
    for (uint256 i = 0; i < length; ++i) {
        IVault strategy = $._strategies[i];
        uint256 strategyBalance = strategy.balanceOf(address(this));
        if (strategyBalance == 0) continue;
        total += strategy.convertToAssets(strategyBalance);
    }
}
```

### _getVeryLiquidVaultStorage()

- **Kind**: internal
- **Source**: 1874:183:145
- **Link**: `src/VeryLiquidVault.sol:VeryLiquidVault:_getVeryLiquidVaultStorage()`

```solidity
function _getVeryLiquidVaultStorage() private pure returns (VeryLiquidVaultStorage storage $) {
    assembly {
        $.slot := VeryLiquidVaultStorageLocation
    }
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

### _setHighWaterMark(uint256)

- **Kind**: internal
- **Source**: 6061:313:151
- **Link**: `src/utils/PerformanceVault.sol:PerformanceVault:_setHighWaterMark(uint256)`

```solidity
/// @notice Sets the high water mark
///  @param highWaterMark_ The new high water mark
function _setHighWaterMark(uint256 highWaterMark_) internal {
    PerformanceVaultStorage storage $ = _getPerformanceVaultStorage();
    uint256 highWaterMarkBefore = $._highWaterMark;
    $._highWaterMark = highWaterMark_;
    emit HighWaterMarkUpdated(highWaterMarkBefore, highWaterMark_);
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

### _addStrategy(contract IVault,address,address)

- **Kind**: internal
- **Source**: 17386:661:145
- **Link**: `src/VeryLiquidVault.sol:VeryLiquidVault:_addStrategy(contract IVault,address,address)`

```solidity
/// @notice Internal function to add a strategy
///  @param strategy_ The strategy to add
///  @param asset_ The asset of the strategy
///  @param auth_ The auth of the strategy
///  @dev Strategy configuration is assumed to be correct (non-malicious, no circular dependencies, etc.)
function _addStrategy(IVault strategy_, address asset_, address auth_) private {
    if (address(strategy_) == address(0)) revert NullAddress();
    if (_isStrategy(strategy_)) revert InvalidStrategy(address(strategy_));
    if ((strategy_.asset() != asset_) || (address(strategy_.auth()) != auth_)) {
        revert InvalidStrategy(address(strategy_));
    }
    VeryLiquidVaultStorage storage $ = _getVeryLiquidVaultStorage();
    $._strategies.push(strategy_);
    emit StrategyAdded(address(strategy_));
    if ($._strategies.length > MAX_STRATEGIES) revert MaxStrategiesExceeded($._strategies.length, MAX_STRATEGIES);
}
```

### _isStrategy(contract IVault)

- **Kind**: internal
- **Source**: 25758:331:145
- **Link**: `src/VeryLiquidVault.sol:VeryLiquidVault:_isStrategy(contract IVault)`

```solidity
/// @notice Internal function to check if the strategy is in the vault
function _isStrategy(IVault strategy) private view returns (bool) {
    VeryLiquidVaultStorage storage $ = _getVeryLiquidVaultStorage();
    uint256 length = $._strategies.length;
    for (uint256 i = 0; i < length; ++i) {
        if ($._strategies[i] == strategy) return true;
    }
    return false;
}
```

### _setRebalanceMaxSlippagePercent(uint256)

- **Kind**: internal
- **Source**: 19041:543:145
- **Link**: `src/VeryLiquidVault.sol:VeryLiquidVault:_setRebalanceMaxSlippagePercent(uint256)`

```solidity
/// @notice Internal function to set the default max slippage percent
///  @param rebalanceMaxSlippagePercent_ The new rebalance max slippage percent
function _setRebalanceMaxSlippagePercent(uint256 rebalanceMaxSlippagePercent_) private {
    if (rebalanceMaxSlippagePercent_ > PERCENT) revert InvalidMaxSlippagePercent(rebalanceMaxSlippagePercent_);
    VeryLiquidVaultStorage storage $ = _getVeryLiquidVaultStorage();
    uint256 oldRebalanceMaxSlippagePercent = $._rebalanceMaxSlippagePercent;
    $._rebalanceMaxSlippagePercent = rebalanceMaxSlippagePercent_;
    emit RebalanceMaxSlippagePercentSet(oldRebalanceMaxSlippagePercent, rebalanceMaxSlippagePercent_);
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
- **Source**: 4944:175:145
- **Link**: `src/VeryLiquidVault.sol:VeryLiquidVault:maxDeposit(address)`

```solidity
/// @inheritdoc ERC4626Upgradeable
///  @dev The maximum amount that can be deposited is the minimum between this receiver specific limit and the maximum asset amount that can be deposited to all strategies
function maxDeposit(address receiver) override(BaseVault) public view returns (uint256) {
    return Math.min(_maxDepositToStrategies(), super.maxDeposit(receiver));
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

### _maxDepositToStrategies()

- **Kind**: internal
- **Source**: 20365:414:145
- **Link**: `src/VeryLiquidVault.sol:VeryLiquidVault:_maxDepositToStrategies()`

```solidity
/// @notice Internal function to calculate maximum depositable amount in all strategies
///  @dev The maximum amount that can be deposited to all strategies is the sum of the maximum amount that can be deposited to each strategy
///  @dev This value might be overstated if nested strategies are used. For example, if a very liquid has two strategies, one of which is an ERC4626StrategyVault and the other is a VeryLiquidVault that has the same ERC4626StrategyVault instance. In this scenario, if the ERC-4626 strategy has 100 maxDeposit remaining, the top-level very liquid would double count this value and return 200. However, in practice, trying to deposit 200 would cause a revert, because only 100 can be deposited.
function _maxDepositToStrategies() private view returns (uint256 maxAssets) {
    VeryLiquidVaultStorage storage $ = _getVeryLiquidVaultStorage();
    uint256 length = $._strategies.length;
    for (uint256 i = 0; i < length; ++i) {
        maxAssets = Math.saturatingAdd(maxAssets, $._strategies[i].maxDeposit(address(this)));
        if (maxAssets == type(uint256).max) break;
    }
}
```

### saturatingAdd(uint256,uint256)

- **Kind**: internal
- **Source**: 3912:199:109
- **Link**: `lib/openzeppelin-contracts/contracts/utils/math/Math.sol:Math:saturatingAdd(uint256,uint256)`

```solidity
///  @dev Unsigned saturating addition, bounds to `2²⁵⁶ - 1` instead of overflowing.
function saturatingAdd(uint256 a, uint256 b) internal pure returns (uint256) {
    (bool success, uint256 result) = tryAdd(a, b);
    return ternary(success, result, type(uint256).max);
}
```

### tryAdd(uint256,uint256)

- **Kind**: internal
- **Source**: 1693:240:109
- **Link**: `lib/openzeppelin-contracts/contracts/utils/math/Math.sol:Math:tryAdd(uint256,uint256)`

```solidity
///  @dev Returns the addition of two unsigned integers, with a success flag (no overflow).
function tryAdd(uint256 a, uint256 b) internal pure returns (bool success, uint256 result) {
    unchecked {
        uint256 c = a + b;
        success = c >= a;
        result = c * SafeCast.toUint(success);
    }
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

### _deposit(address,address,uint256,uint256)

- **Kind**: internal
- **Source**: 7830:1408:145
- **Link**: `src/VeryLiquidVault.sol:VeryLiquidVault:_deposit(address,address,uint256,uint256)`

```solidity
/// @inheritdoc ERC4626Upgradeable
///  @dev Tries to deposit to strategies sequentially, reverts if not all assets can be deposited
function _deposit(address caller, address receiver, uint256 assets, uint256 shares) override internal {
    if (_isInitializing()) {
        shares = assets;
    }
    super._deposit(caller, receiver, assets, shares);
    uint256 assetsToDeposit = assets;
    VeryLiquidVaultStorage storage $ = _getVeryLiquidVaultStorage();
    uint256 length = $._strategies.length;
    for (uint256 i = 0; i < length; ++i) {
        if (assetsToDeposit == 0) break;
        IVault strategy = $._strategies[i];
        uint256 strategyMaxDeposit = strategy.maxDeposit(address(this));
        uint256 depositAmount = Math.min(assetsToDeposit, strategyMaxDeposit);
        if (depositAmount > 0) {
            IERC20(asset()).forceApprove(address(strategy), depositAmount);
            try strategy.deposit(depositAmount, address(this)) {
                assetsToDeposit -= depositAmount;
            } catch {
                emit DepositFailed(address(strategy), depositAmount);
                IERC20(asset()).forceApprove(address(strategy), 0);
            }
        }
    }
    if (assetsToDeposit > 0) revert CannotDepositToStrategies(assets, shares, assetsToDeposit);
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

- **Auth::getRoleMember(bytes32,uint256)**

## State Variable Reads

- **DEFAULT_MAX_SLIPPAGE_PERCENT** (`uint256`)
- **MAXIMUM_PERFORMANCE_FEE_PERCENT** (`uint256`)
- **INITIALIZABLE_STORAGE** (`bytes32`)
- **MAX_STRATEGIES** (`uint256`)
- **NOT_ENTERED** (`uint256`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: VeryLiquidVault.initialize(contract Auth,contract IERC20,string,string,address,uint256,contract IVault[]) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
  ├─ [1] ⚙️ FUNCTION: PerformanceVault.__PerformanceVault_init(address,uint256) (NodeID: 1)
  │   💬 Args: [auth_.getRoleMember(DEFAULT_ADMIN_ROLE, 0), 0]
  │   👁️  Def: internal
  │ ├─ [2] ⚙️ FUNCTION: PerformanceVault._setFeeRecipient(address) (NodeID: 2)
  │ │   💬 Args: [feeRecipient_]
  │ │   👁️  Def: internal
  │ │ └─ [3] ⚙️ FUNCTION: PerformanceVault._getPerformanceVaultStorage() (NodeID: 3)
  │ │     💬 Args: [no args]
  │ │     👁️  Def: private
  │ ├─ [2] ⚙️ FUNCTION: PerformanceVault._setPerformanceFeePercent(uint256) (NodeID: 4)
  │ │   💬 Args: [performanceFeePercent_]
  │ │   👁️  Def: internal
  │ │ ├─ [3] ⚙️ FUNCTION: PerformanceVault._getPerformanceVaultStorage() (NodeID: 5)
  │ │ │   💬 Args: [no args]
  │ │ │   👁️  Def: private
  │ │ ├─ [3] ⚙️ FUNCTION: PerformanceVault._pps() (NodeID: 6)
  │ │ │   💬 Args: [no args]
  │ │ │   👁️  Def: private
  │ │ │ ├─ [4] ⚙️ FUNCTION: VeryLiquidVault.totalAssets() (NodeID: 7)
  │ │ │ │   💬 Args: [no args]
  │ │ │ │   👁️  Def: public
  │ │ │ │ └─ [5] ⚙️ FUNCTION: VeryLiquidVault._getVeryLiquidVaultStorage() (NodeID: 8)
  │ │ │ │     💬 Args: [no args]
  │ │ │ │     👁️  Def: private
  │ │ │ ├─ [4] ⚙️ FUNCTION: ERC20Upgradeable.totalSupply() (NodeID: 9)
  │ │ │ │   💬 Args: [no args]
  │ │ │ │   👁️  Def: public
  │ │ │ │ └─ [5] ⚙️ FUNCTION: ERC20Upgradeable._getERC20Storage() (NodeID: 10)
  │ │ │ │     💬 Args: [no args]
  │ │ │ │     👁️  Def: private
  │ │ │ └─ [4] ⚙️ FUNCTION: Math.mulDiv(uint256,uint256,uint256) (NodeID: 11)
  │ │ │     💬 Args: [totalAssets_, PERCENT, totalSupply_]
  │ │ │     👁️  Def: internal
  │ │ │   ├─ [5] ⚙️ FUNCTION: Math.mul512(uint256,uint256) (NodeID: 12)
  │ │ │   │   💬 Args: [x, y]
  │ │ │   │   👁️  Def: internal
  │ │ │   └─ [5] ⚙️ FUNCTION: Panic.panic(uint256) (NodeID: 13)
  │ │ │       💬 Args: [ternary(denominator == 0, Panic.DIVISION_BY_ZERO, Panic.UNDER_OVERFLOW)]
  │ │ │       👁️  Def: internal
  │ │ │     └─ [6] ⚙️ FUNCTION: Math.ternary(bool,uint256,uint256) (NodeID: 14)
  │ │ │         💬 Args: [denominator == 0, Panic.DIVISION_BY_ZERO, Panic.UNDER_OVERFLOW]
  │ │ │         👁️  Def: internal
  │ │ │       └─ [7] ⚙️ FUNCTION: SafeCast.toUint(bool) (NodeID: 15)
  │ │ │           💬 Args: [condition]
  │ │ │           👁️  Def: internal
  │ │ └─ [3] ⚙️ FUNCTION: PerformanceVault._setHighWaterMark(uint256) (NodeID: 16)
  │ │     💬 Args: [currentPPS]
  │ │     👁️  Def: internal
  │ │   └─ [4] ⚙️ FUNCTION: PerformanceVault._getPerformanceVaultStorage() (NodeID: 17)
  │ │       💬 Args: [no args]
  │ │       👁️  Def: private
  │ └─ [2] 🔒 MODIFIER: Initializable.onlyInitializing() (NodeID: 18)
  │     💬 Args: [no args]
  │   └─ [3] ⚙️ FUNCTION: Initializable._checkInitializing() (NodeID: 19)
  │       💬 Args: [no args]
  │       👁️  Def: internal
  │     └─ [4] ⚙️ FUNCTION: Initializable._isInitializing() (NodeID: 20)
  │         💬 Args: [no args]
  │         👁️  Def: internal
  │       └─ [5] ⚙️ FUNCTION: Initializable._getInitializableStorage() (NodeID: 21)
  │           💬 Args: [no args]
  │           👁️  Def: private
  │         └─ [6] ⚙️ FUNCTION: Initializable._initializableStorageSlot() (NodeID: 22)
  │             💬 Args: [no args]
  │             👁️  Def: internal
  ├─ [1] ⚙️ FUNCTION: VeryLiquidVault._addStrategy(contract IVault,address,address) (NodeID: 23)
  │   💬 Args: [strategies_[i], address(asset_), address(auth_)]
  │   👁️  Def: private
  │ ├─ [2] ⚙️ FUNCTION: VeryLiquidVault._isStrategy(contract IVault) (NodeID: 24)
  │ │   💬 Args: [strategy_]
  │ │   👁️  Def: private
  │ │ └─ [3] ⚙️ FUNCTION: VeryLiquidVault._getVeryLiquidVaultStorage() (NodeID: 25)
  │ │     💬 Args: [no args]
  │ │     👁️  Def: private
  │ └─ [2] ⚙️ FUNCTION: VeryLiquidVault._getVeryLiquidVaultStorage() (NodeID: 26)
  │     💬 Args: [no args]
  │     👁️  Def: private
  ├─ [1] ⚙️ FUNCTION: VeryLiquidVault._setRebalanceMaxSlippagePercent(uint256) (NodeID: 27)
  │   💬 Args: [DEFAULT_MAX_SLIPPAGE_PERCENT]
  │   👁️  Def: private
  │ └─ [2] ⚙️ FUNCTION: VeryLiquidVault._getVeryLiquidVaultStorage() (NodeID: 28)
  │     💬 Args: [no args]
  │     👁️  Def: private
  ├─ [1] ⚙️ FUNCTION: BaseVault.initialize(contract Auth,contract IERC20,string,string,address,uint256) (NodeID: 29)
  │   💬 Args: [auth_, asset_, name_, symbol_, fundingAccount, firstDepositAmount]
  │   👁️  Def: public
  │ ├─ [2] ⚙️ FUNCTION: ERC4626Upgradeable.__ERC4626_init(contract IERC20) (NodeID: 30)
  │ │   💬 Args: [asset_]
  │ │   👁️  Def: internal
  │ │ ├─ [3] ⚙️ FUNCTION: ERC4626Upgradeable.__ERC4626_init_unchained(contract IERC20) (NodeID: 31)
  │ │ │   💬 Args: [asset_]
  │ │ │   👁️  Def: internal
  │ │ │ ├─ [4] ⚙️ FUNCTION: ERC4626Upgradeable._getERC4626Storage() (NodeID: 32)
  │ │ │ │   💬 Args: [no args]
  │ │ │ │   👁️  Def: private
  │ │ │ ├─ [4] ⚙️ FUNCTION: ERC4626Upgradeable._tryGetAssetDecimals(contract IERC20) (NodeID: 33)
  │ │ │ │   💬 Args: [asset_]
  │ │ │ │   👁️  Def: private
  │ │ │ └─ [4] 🔒 MODIFIER: Initializable.onlyInitializing() (NodeID: 34)
  │ │ │     💬 Args: [no args]
  │ │ │   └─ [5] ⚙️ FUNCTION: Initializable._checkInitializing() (NodeID: 35)
  │ │ │       💬 Args: [no args]
  │ │ │       👁️  Def: internal
  │ │ │     └─ [6] ⚙️ FUNCTION: Initializable._isInitializing() (NodeID: 36)
  │ │ │         💬 Args: [no args]
  │ │ │         👁️  Def: internal
  │ │ │       └─ [7] ⚙️ FUNCTION: Initializable._getInitializableStorage() (NodeID: 37)
  │ │ │           💬 Args: [no args]
  │ │ │           👁️  Def: private
  │ │ │         └─ [8] ⚙️ FUNCTION: Initializable._initializableStorageSlot() (NodeID: 38)
  │ │ │             💬 Args: [no args]
  │ │ │             👁️  Def: internal
  │ │ └─ [3] 🔒 MODIFIER: Initializable.onlyInitializing() (NodeID: 39)
  │ │     💬 Args: [no args]
  │ │   └─ [4] ⚙️ FUNCTION: Initializable._checkInitializing() (NodeID: 40)
  │ │       💬 Args: [no args]
  │ │       👁️  Def: internal
  │ │     └─ [5] ⚙️ FUNCTION: Initializable._isInitializing() (NodeID: 41)
  │ │         💬 Args: [no args]
  │ │         👁️  Def: internal
  │ │       └─ [6] ⚙️ FUNCTION: Initializable._getInitializableStorage() (NodeID: 42)
  │ │           💬 Args: [no args]
  │ │           👁️  Def: private
  │ │         └─ [7] ⚙️ FUNCTION: Initializable._initializableStorageSlot() (NodeID: 43)
  │ │             💬 Args: [no args]
  │ │             👁️  Def: internal
  │ ├─ [2] ⚙️ FUNCTION: ERC20Upgradeable.__ERC20_init(string,string) (NodeID: 44)
  │ │   💬 Args: [name_, symbol_]
  │ │   👁️  Def: internal
  │ │ ├─ [3] ⚙️ FUNCTION: ERC20Upgradeable.__ERC20_init_unchained(string,string) (NodeID: 45)
  │ │ │   💬 Args: [name_, symbol_]
  │ │ │   👁️  Def: internal
  │ │ │ ├─ [4] ⚙️ FUNCTION: ERC20Upgradeable._getERC20Storage() (NodeID: 46)
  │ │ │ │   💬 Args: [no args]
  │ │ │ │   👁️  Def: private
  │ │ │ └─ [4] 🔒 MODIFIER: Initializable.onlyInitializing() (NodeID: 47)
  │ │ │     💬 Args: [no args]
  │ │ │   └─ [5] ⚙️ FUNCTION: Initializable._checkInitializing() (NodeID: 48)
  │ │ │       💬 Args: [no args]
  │ │ │       👁️  Def: internal
  │ │ │     └─ [6] ⚙️ FUNCTION: Initializable._isInitializing() (NodeID: 49)
  │ │ │         💬 Args: [no args]
  │ │ │         👁️  Def: internal
  │ │ │       └─ [7] ⚙️ FUNCTION: Initializable._getInitializableStorage() (NodeID: 50)
  │ │ │           💬 Args: [no args]
  │ │ │           👁️  Def: private
  │ │ │         └─ [8] ⚙️ FUNCTION: Initializable._initializableStorageSlot() (NodeID: 51)
  │ │ │             💬 Args: [no args]
  │ │ │             👁️  Def: internal
  │ │ └─ [3] 🔒 MODIFIER: Initializable.onlyInitializing() (NodeID: 52)
  │ │     💬 Args: [no args]
  │ │   └─ [4] ⚙️ FUNCTION: Initializable._checkInitializing() (NodeID: 53)
  │ │       💬 Args: [no args]
  │ │       👁️  Def: internal
  │ │     └─ [5] ⚙️ FUNCTION: Initializable._isInitializing() (NodeID: 54)
  │ │         💬 Args: [no args]
  │ │         👁️  Def: internal
  │ │       └─ [6] ⚙️ FUNCTION: Initializable._getInitializableStorage() (NodeID: 55)
  │ │           💬 Args: [no args]
  │ │           👁️  Def: private
  │ │         └─ [7] ⚙️ FUNCTION: Initializable._initializableStorageSlot() (NodeID: 56)
  │ │             💬 Args: [no args]
  │ │             👁️  Def: internal
  │ ├─ [2] ⚙️ FUNCTION: ERC20PermitUpgradeable.__ERC20Permit_init(string) (NodeID: 57)
  │ │   💬 Args: [name_]
  │ │   👁️  Def: internal
  │ │ ├─ [3] ⚙️ FUNCTION: EIP712Upgradeable.__EIP712_init_unchained(string,string) (NodeID: 58)
  │ │ │   💬 Args: [name, "1"]
  │ │ │   👁️  Def: internal
  │ │ │ ├─ [4] ⚙️ FUNCTION: EIP712Upgradeable._getEIP712Storage() (NodeID: 59)
  │ │ │ │   💬 Args: [no args]
  │ │ │ │   👁️  Def: private
  │ │ │ └─ [4] 🔒 MODIFIER: Initializable.onlyInitializing() (NodeID: 60)
  │ │ │     💬 Args: [no args]
  │ │ │   └─ [5] ⚙️ FUNCTION: Initializable._checkInitializing() (NodeID: 61)
  │ │ │       💬 Args: [no args]
  │ │ │       👁️  Def: internal
  │ │ │     └─ [6] ⚙️ FUNCTION: Initializable._isInitializing() (NodeID: 62)
  │ │ │         💬 Args: [no args]
  │ │ │         👁️  Def: internal
  │ │ │       └─ [7] ⚙️ FUNCTION: Initializable._getInitializableStorage() (NodeID: 63)
  │ │ │           💬 Args: [no args]
  │ │ │           👁️  Def: private
  │ │ │         └─ [8] ⚙️ FUNCTION: Initializable._initializableStorageSlot() (NodeID: 64)
  │ │ │             💬 Args: [no args]
  │ │ │             👁️  Def: internal
  │ │ └─ [3] 🔒 MODIFIER: Initializable.onlyInitializing() (NodeID: 65)
  │ │     💬 Args: [no args]
  │ │   └─ [4] ⚙️ FUNCTION: Initializable._checkInitializing() (NodeID: 66)
  │ │       💬 Args: [no args]
  │ │       👁️  Def: internal
  │ │     └─ [5] ⚙️ FUNCTION: Initializable._isInitializing() (NodeID: 67)
  │ │         💬 Args: [no args]
  │ │         👁️  Def: internal
  │ │       └─ [6] ⚙️ FUNCTION: Initializable._getInitializableStorage() (NodeID: 68)
  │ │           💬 Args: [no args]
  │ │           👁️  Def: private
  │ │         └─ [7] ⚙️ FUNCTION: Initializable._initializableStorageSlot() (NodeID: 69)
  │ │             💬 Args: [no args]
  │ │             👁️  Def: internal
  │ ├─ [2] ⚙️ FUNCTION: ReentrancyGuardUpgradeable.__ReentrancyGuard_init() (NodeID: 70)
  │ │   💬 Args: [no args]
  │ │   👁️  Def: internal
  │ │ ├─ [3] ⚙️ FUNCTION: ReentrancyGuardUpgradeable.__ReentrancyGuard_init_unchained() (NodeID: 71)
  │ │ │   💬 Args: [no args]
  │ │ │   👁️  Def: internal
  │ │ │ ├─ [4] ⚙️ FUNCTION: ReentrancyGuardUpgradeable._getReentrancyGuardStorage() (NodeID: 72)
  │ │ │ │   💬 Args: [no args]
  │ │ │ │   👁️  Def: private
  │ │ │ └─ [4] 🔒 MODIFIER: Initializable.onlyInitializing() (NodeID: 73)
  │ │ │     💬 Args: [no args]
  │ │ │   └─ [5] ⚙️ FUNCTION: Initializable._checkInitializing() (NodeID: 74)
  │ │ │       💬 Args: [no args]
  │ │ │       👁️  Def: internal
  │ │ │     └─ [6] ⚙️ FUNCTION: Initializable._isInitializing() (NodeID: 75)
  │ │ │         💬 Args: [no args]
  │ │ │         👁️  Def: internal
  │ │ │       └─ [7] ⚙️ FUNCTION: Initializable._getInitializableStorage() (NodeID: 76)
  │ │ │           💬 Args: [no args]
  │ │ │           👁️  Def: private
  │ │ │         └─ [8] ⚙️ FUNCTION: Initializable._initializableStorageSlot() (NodeID: 77)
  │ │ │             💬 Args: [no args]
  │ │ │             👁️  Def: internal
  │ │ └─ [3] 🔒 MODIFIER: Initializable.onlyInitializing() (NodeID: 78)
  │ │     💬 Args: [no args]
  │ │   └─ [4] ⚙️ FUNCTION: Initializable._checkInitializing() (NodeID: 79)
  │ │       💬 Args: [no args]
  │ │       👁️  Def: internal
  │ │     └─ [5] ⚙️ FUNCTION: Initializable._isInitializing() (NodeID: 80)
  │ │         💬 Args: [no args]
  │ │         👁️  Def: internal
  │ │       └─ [6] ⚙️ FUNCTION: Initializable._getInitializableStorage() (NodeID: 81)
  │ │           💬 Args: [no args]
  │ │           👁️  Def: private
  │ │         └─ [7] ⚙️ FUNCTION: Initializable._initializableStorageSlot() (NodeID: 82)
  │ │             💬 Args: [no args]
  │ │             👁️  Def: internal
  │ ├─ [2] ⚙️ FUNCTION: PausableUpgradeable.__Pausable_init() (NodeID: 83)
  │ │   💬 Args: [no args]
  │ │   👁️  Def: internal
  │ │ └─ [3] 🔒 MODIFIER: Initializable.onlyInitializing() (NodeID: 84)
  │ │     💬 Args: [no args]
  │ │   └─ [4] ⚙️ FUNCTION: Initializable._checkInitializing() (NodeID: 85)
  │ │       💬 Args: [no args]
  │ │       👁️  Def: internal
  │ │     └─ [5] ⚙️ FUNCTION: Initializable._isInitializing() (NodeID: 86)
  │ │         💬 Args: [no args]
  │ │         👁️  Def: internal
  │ │       └─ [6] ⚙️ FUNCTION: Initializable._getInitializableStorage() (NodeID: 87)
  │ │           💬 Args: [no args]
  │ │           👁️  Def: private
  │ │         └─ [7] ⚙️ FUNCTION: Initializable._initializableStorageSlot() (NodeID: 88)
  │ │             💬 Args: [no args]
  │ │             👁️  Def: internal
  │ ├─ [2] ⚙️ FUNCTION: MulticallUpgradeable.__Multicall_init() (NodeID: 89)
  │ │   💬 Args: [no args]
  │ │   👁️  Def: internal
  │ │ └─ [3] 🔒 MODIFIER: Initializable.onlyInitializing() (NodeID: 90)
  │ │     💬 Args: [no args]
  │ │   └─ [4] ⚙️ FUNCTION: Initializable._checkInitializing() (NodeID: 91)
  │ │       💬 Args: [no args]
  │ │       👁️  Def: internal
  │ │     └─ [5] ⚙️ FUNCTION: Initializable._isInitializing() (NodeID: 92)
  │ │         💬 Args: [no args]
  │ │         👁️  Def: internal
  │ │       └─ [6] ⚙️ FUNCTION: Initializable._getInitializableStorage() (NodeID: 93)
  │ │           💬 Args: [no args]
  │ │           👁️  Def: private
  │ │         └─ [7] ⚙️ FUNCTION: Initializable._initializableStorageSlot() (NodeID: 94)
  │ │             💬 Args: [no args]
  │ │             👁️  Def: internal
  │ ├─ [2] ⚙️ FUNCTION: UUPSUpgradeable.__UUPSUpgradeable_init() (NodeID: 95)
  │ │   💬 Args: [no args]
  │ │   👁️  Def: internal
  │ │ └─ [3] 🔒 MODIFIER: Initializable.onlyInitializing() (NodeID: 96)
  │ │     💬 Args: [no args]
  │ │   └─ [4] ⚙️ FUNCTION: Initializable._checkInitializing() (NodeID: 97)
  │ │       💬 Args: [no args]
  │ │       👁️  Def: internal
  │ │     └─ [5] ⚙️ FUNCTION: Initializable._isInitializing() (NodeID: 98)
  │ │         💬 Args: [no args]
  │ │         👁️  Def: internal
  │ │       └─ [6] ⚙️ FUNCTION: Initializable._getInitializableStorage() (NodeID: 99)
  │ │           💬 Args: [no args]
  │ │           👁️  Def: private
  │ │         └─ [7] ⚙️ FUNCTION: Initializable._initializableStorageSlot() (NodeID: 100)
  │ │             💬 Args: [no args]
  │ │             👁️  Def: internal
  │ ├─ [2] ⚙️ FUNCTION: BaseVault._getBaseVaultStorage() (NodeID: 101)
  │ │   💬 Args: [no args]
  │ │   👁️  Def: private
  │ ├─ [2] ⚙️ FUNCTION: BaseVault._setTotalAssetsCap(uint256) (NodeID: 102)
  │ │   💬 Args: [type(uint256).max]
  │ │   👁️  Def: private
  │ │ └─ [3] ⚙️ FUNCTION: BaseVault._getBaseVaultStorage() (NodeID: 103)
  │ │     💬 Args: [no args]
  │ │     👁️  Def: private
  │ ├─ [2] ⚙️ FUNCTION: BaseVault._firstDeposit(address,uint256) (NodeID: 104)
  │ │   💬 Args: [fundingAccount_, firstDepositAmount_]
  │ │   👁️  Def: private
  │ │ ├─ [3] ⚙️ FUNCTION: VeryLiquidVault.maxDeposit(address) (NodeID: 105)
  │ │ │   💬 Args: [receiver]
  │ │ │   👁️  Def: public
  │ │ │ └─ [4] ⚙️ FUNCTION: Math.min(uint256,uint256) (NodeID: 106)
  │ │ │     💬 Args: [_maxDepositToStrategies(), super.maxDeposit(receiver)]
  │ │ │     👁️  Def: internal
  │ │ │   ├─ [5] ⚙️ FUNCTION: VeryLiquidVault._maxDepositToStrategies() (NodeID: 109)
  │ │ │   │   💬 Args: [no args]
  │ │ │   │   👁️  Def: private
  │ │ │   │ ├─ [6] ⚙️ FUNCTION: VeryLiquidVault._getVeryLiquidVaultStorage() (NodeID: 110)
  │ │ │   │ │   💬 Args: [no args]
  │ │ │   │ │   👁️  Def: private
  │ │ │   │ └─ [6] ⚙️ FUNCTION: Math.saturatingAdd(uint256,uint256) (NodeID: 111)
  │ │ │   │     💬 Args: [maxAssets, $._strategies[i].maxDeposit(address(this))]
  │ │ │   │     👁️  Def: internal
  │ │ │   │   ├─ [7] ⚙️ FUNCTION: Math.tryAdd(uint256,uint256) (NodeID: 112)
  │ │ │   │   │   💬 Args: [a, b]
  │ │ │   │   │   👁️  Def: internal
  │ │ │   │   │ └─ [8] ⚙️ FUNCTION: SafeCast.toUint(bool) (NodeID: 113)
  │ │ │   │   │     💬 Args: [success]
  │ │ │   │   │     👁️  Def: internal
  │ │ │   │   └─ [7] ⚙️ FUNCTION: Math.ternary(bool,uint256,uint256) (NodeID: 114)
  │ │ │   │       💬 Args: [success, result, type(uint256).max]
  │ │ │   │       👁️  Def: internal
  │ │ │   │     └─ [8] ⚙️ FUNCTION: SafeCast.toUint(bool) (NodeID: 115)
  │ │ │   │         💬 Args: [condition]
  │ │ │   │         👁️  Def: internal
  │ │ │   ├─ [5] ⚙️ FUNCTION: BaseVault.maxDeposit(address) (NodeID: 116)
  │ │ │   │   💬 Args: [receiver]
  │ │ │   │   👁️  Def: public
  │ │ │   │ ├─ [6] ⚙️ FUNCTION: BaseVault._pausedOrAuthPaused() (NodeID: 117)
  │ │ │   │ │   💬 Args: [no args]
  │ │ │   │ │   👁️  Def: private
  │ │ │   │ │ ├─ [7] ⚙️ FUNCTION: BaseVault.auth() (NodeID: 118)
  │ │ │   │ │ │   💬 Args: [no args]
  │ │ │   │ │ │   👁️  Def: public
  │ │ │   │ │ │ └─ [8] ⚙️ FUNCTION: BaseVault._getBaseVaultStorage() (NodeID: 119)
  │ │ │   │ │ │     💬 Args: [no args]
  │ │ │   │ │ │     👁️  Def: private
  │ │ │   │ │ └─ [7] ⚙️ FUNCTION: PausableUpgradeable.paused() (NodeID: 120)
  │ │ │   │ │     💬 Args: [no args]
  │ │ │   │ │     👁️  Def: public
  │ │ │   │ │   └─ [8] ⚙️ FUNCTION: PausableUpgradeable._getPausableStorage() (NodeID: 121)
  │ │ │   │ │       💬 Args: [no args]
  │ │ │   │ │       👁️  Def: private
  │ │ │   │ ├─ [6] ⚙️ FUNCTION: BaseVault._totalAssetsCap() (NodeID: 122)
  │ │ │   │ │   💬 Args: [no args]
  │ │ │   │ │   👁️  Def: private
  │ │ │   │ │ └─ [7] ⚙️ FUNCTION: BaseVault._getBaseVaultStorage() (NodeID: 123)
  │ │ │   │ │     💬 Args: [no args]
  │ │ │   │ │     👁️  Def: private
  │ │ │   │ ├─ [6] ⚙️ FUNCTION: ERC4626Upgradeable.maxDeposit(address) (NodeID: 124)
  │ │ │   │ │   💬 Args: [receiver]
  │ │ │   │ │   👁️  Def: public
  │ │ │   │ └─ [6] ⚙️ FUNCTION: BaseVault._maxDeposit() (NodeID: 125)
  │ │ │   │     💬 Args: [no args]
  │ │ │   │     👁️  Def: private
  │ │ │   │   └─ [7] ⚙️ FUNCTION: Math.saturatingSub(uint256,uint256) (NodeID: 126)
  │ │ │   │       💬 Args: [_totalAssetsCap(), totalAssets()]
  │ │ │   │       👁️  Def: internal
  │ │ │   │     ├─ [8] ⚙️ FUNCTION: BaseVault._totalAssetsCap() (NodeID: 129)
  │ │ │   │     │   💬 Args: [no args]
  │ │ │   │     │   👁️  Def: private
  │ │ │   │     │ └─ [9] ⚙️ FUNCTION: BaseVault._getBaseVaultStorage() (NodeID: 130)
  │ │ │   │     │     💬 Args: [no args]
  │ │ │   │     │     👁️  Def: private
  │ │ │   │     ├─ [8] ⚙️ FUNCTION: VeryLiquidVault.totalAssets() (NodeID: 131)
  │ │ │   │     │   💬 Args: [no args]
  │ │ │   │     │   👁️  Def: public
  │ │ │   │     │ └─ [9] ⚙️ FUNCTION: VeryLiquidVault._getVeryLiquidVaultStorage() (NodeID: 132)
  │ │ │   │     │     💬 Args: [no args]
  │ │ │   │     │     👁️  Def: private
  │ │ │   │     └─ [8] ⚙️ FUNCTION: Math.trySub(uint256,uint256) (NodeID: 127)
  │ │ │   │         💬 Args: [a, b]
  │ │ │   │         👁️  Def: internal
  │ │ │   │       └─ [9] ⚙️ FUNCTION: SafeCast.toUint(bool) (NodeID: 128)
  │ │ │   │           💬 Args: [success]
  │ │ │   │           👁️  Def: internal
  │ │ │   └─ [5] ⚙️ FUNCTION: Math.ternary(bool,uint256,uint256) (NodeID: 107)
  │ │ │       💬 Args: [a < b, a, b]
  │ │ │       👁️  Def: internal
  │ │ │     └─ [6] ⚙️ FUNCTION: SafeCast.toUint(bool) (NodeID: 108)
  │ │ │         💬 Args: [condition]
  │ │ │         👁️  Def: internal
  │ │ ├─ [3] ⚙️ FUNCTION: ERC4626Upgradeable.previewDeposit(uint256) (NodeID: 133)
  │ │ │   💬 Args: [firstDepositAmount_]
  │ │ │   👁️  Def: public
  │ │ │ └─ [4] ⚙️ FUNCTION: ERC4626Upgradeable._convertToShares(uint256,enum Math.Rounding) (NodeID: 134)
  │ │ │     💬 Args: [assets, Math.Rounding.Floor]
  │ │ │     👁️  Def: internal
  │ │ │   └─ [5] ⚙️ FUNCTION: Math.mulDiv(uint256,uint256,uint256,enum Math.Rounding) (NodeID: 135)
  │ │ │       💬 Args: [assets, totalSupply() + (10 ** _decimalsOffset()), totalAssets() + 1, rounding]
  │ │ │       👁️  Def: internal
  │ │ │     ├─ [6] ⚙️ FUNCTION: ERC4626Upgradeable._decimalsOffset() (NodeID: 143)
  │ │ │     │   💬 Args: [no args]
  │ │ │     │   👁️  Def: internal
  │ │ │     ├─ [6] ⚙️ FUNCTION: ERC20Upgradeable.totalSupply() (NodeID: 144)
  │ │ │     │   💬 Args: [no args]
  │ │ │     │   👁️  Def: public
  │ │ │     │ └─ [7] ⚙️ FUNCTION: ERC20Upgradeable._getERC20Storage() (NodeID: 145)
  │ │ │     │     💬 Args: [no args]
  │ │ │     │     👁️  Def: private
  │ │ │     ├─ [6] ⚙️ FUNCTION: VeryLiquidVault.totalAssets() (NodeID: 146)
  │ │ │     │   💬 Args: [no args]
  │ │ │     │   👁️  Def: public
  │ │ │     │ └─ [7] ⚙️ FUNCTION: VeryLiquidVault._getVeryLiquidVaultStorage() (NodeID: 147)
  │ │ │     │     💬 Args: [no args]
  │ │ │     │     👁️  Def: private
  │ │ │     ├─ [6] ⚙️ FUNCTION: SafeCast.toUint(bool) (NodeID: 136)
  │ │ │     │   💬 Args: [unsignedRoundsUp(rounding) && (mulmod(x, y, denominator) > 0)]
  │ │ │     │   👁️  Def: internal
  │ │ │     │ └─ [7] ⚙️ FUNCTION: Math.unsignedRoundsUp(enum Math.Rounding) (NodeID: 137)
  │ │ │     │     💬 Args: [rounding]
  │ │ │     │     👁️  Def: internal
  │ │ │     └─ [6] ⚙️ FUNCTION: Math.mulDiv(uint256,uint256,uint256) (NodeID: 138)
  │ │ │         💬 Args: [x, y, denominator]
  │ │ │         👁️  Def: internal
  │ │ │       ├─ [7] ⚙️ FUNCTION: Math.mul512(uint256,uint256) (NodeID: 139)
  │ │ │       │   💬 Args: [x, y]
  │ │ │       │   👁️  Def: internal
  │ │ │       └─ [7] ⚙️ FUNCTION: Panic.panic(uint256) (NodeID: 140)
  │ │ │           💬 Args: [ternary(denominator == 0, Panic.DIVISION_BY_ZERO, Panic.UNDER_OVERFLOW)]
  │ │ │           👁️  Def: internal
  │ │ │         └─ [8] ⚙️ FUNCTION: Math.ternary(bool,uint256,uint256) (NodeID: 141)
  │ │ │             💬 Args: [denominator == 0, Panic.DIVISION_BY_ZERO, Panic.UNDER_OVERFLOW]
  │ │ │             👁️  Def: internal
  │ │ │           └─ [9] ⚙️ FUNCTION: SafeCast.toUint(bool) (NodeID: 142)
  │ │ │               💬 Args: [condition]
  │ │ │               👁️  Def: internal
  │ │ └─ [3] ⚙️ FUNCTION: VeryLiquidVault._deposit(address,address,uint256,uint256) (NodeID: 148)
  │ │     💬 Args: [fundingAccount_, receiver, firstDepositAmount_, shares]
  │ │     👁️  Def: internal
  │ │   ├─ [4] ⚙️ FUNCTION: Initializable._isInitializing() (NodeID: 149)
  │ │   │   💬 Args: [no args]
  │ │   │   👁️  Def: internal
  │ │   │ └─ [5] ⚙️ FUNCTION: Initializable._getInitializableStorage() (NodeID: 150)
  │ │   │     💬 Args: [no args]
  │ │   │     👁️  Def: private
  │ │   │   └─ [6] ⚙️ FUNCTION: Initializable._initializableStorageSlot() (NodeID: 151)
  │ │   │       💬 Args: [no args]
  │ │   │       👁️  Def: internal
  │ │   ├─ [4] ⚙️ FUNCTION: BaseVault._deposit(address,address,uint256,uint256) (NodeID: 152)
  │ │   │   💬 Args: [caller, receiver, assets, shares]
  │ │   │   👁️  Def: internal
  │ │   │ └─ [5] ⚙️ FUNCTION: ERC4626Upgradeable._deposit(address,address,uint256,uint256) (NodeID: 153)
  │ │   │     💬 Args: [caller, receiver, assets, shares]
  │ │   │     👁️  Def: internal
  │ │   │   ├─ [6] ⚙️ FUNCTION: SafeERC20.safeTransferFrom(contract IERC20,address,address,uint256) (NodeID: 154)
  │ │   │   │   💬 Args: [IERC20(asset()), caller, address(this), assets]
  │ │   │   │   👁️  Def: internal
  │ │   │   │ ├─ [7] ⚙️ FUNCTION: ERC4626Upgradeable.asset() (NodeID: 156)
  │ │   │   │ │   💬 Args: [no args]
  │ │   │   │ │   👁️  Def: public
  │ │   │   │ │ └─ [8] ⚙️ FUNCTION: ERC4626Upgradeable._getERC4626Storage() (NodeID: 157)
  │ │   │   │ │     💬 Args: [no args]
  │ │   │   │ │     👁️  Def: private
  │ │   │   │ └─ [7] ⚙️ FUNCTION: SafeERC20._callOptionalReturn(contract IERC20,bytes) (NodeID: 155)
  │ │   │   │     💬 Args: [token, abi.encodeCall(token.transferFrom, (from, to, value))]
  │ │   │   │     👁️  Def: private
  │ │   │   └─ [6] ⚙️ FUNCTION: ERC20Upgradeable._mint(address,uint256) (NodeID: 158)
  │ │   │       💬 Args: [receiver, shares]
  │ │   │       👁️  Def: internal
  │ │   │     └─ [7] ⚙️ FUNCTION: ERC20Upgradeable._update(address,address,uint256) (NodeID: 159)
  │ │   │         💬 Args: [address(0), account, value]
  │ │   │         👁️  Def: internal
  │ │   │       └─ [8] ⚙️ FUNCTION: ERC20Upgradeable._getERC20Storage() (NodeID: 160)
  │ │   │           💬 Args: [no args]
  │ │   │           👁️  Def: private
  │ │   ├─ [4] ⚙️ FUNCTION: VeryLiquidVault._getVeryLiquidVaultStorage() (NodeID: 161)
  │ │   │   💬 Args: [no args]
  │ │   │   👁️  Def: private
  │ │   ├─ [4] ⚙️ FUNCTION: Math.min(uint256,uint256) (NodeID: 162)
  │ │   │   💬 Args: [assetsToDeposit, strategyMaxDeposit]
  │ │   │   👁️  Def: internal
  │ │   │ └─ [5] ⚙️ FUNCTION: Math.ternary(bool,uint256,uint256) (NodeID: 163)
  │ │   │     💬 Args: [a < b, a, b]
  │ │   │     👁️  Def: internal
  │ │   │   └─ [6] ⚙️ FUNCTION: SafeCast.toUint(bool) (NodeID: 164)
  │ │   │       💬 Args: [condition]
  │ │   │       👁️  Def: internal
  │ │   ├─ [4] ⚙️ FUNCTION: ERC4626Upgradeable.asset() (NodeID: 165)
  │ │   │   💬 Args: [no args]
  │ │   │   👁️  Def: public
  │ │   │ └─ [5] ⚙️ FUNCTION: ERC4626Upgradeable._getERC4626Storage() (NodeID: 166)
  │ │   │     💬 Args: [no args]
  │ │   │     👁️  Def: private
  │ │   └─ [4] ⚙️ FUNCTION: ERC4626Upgradeable.asset() (NodeID: 167)
  │ │       💬 Args: [no args]
  │ │       👁️  Def: public
  │ │     └─ [5] ⚙️ FUNCTION: ERC4626Upgradeable._getERC4626Storage() (NodeID: 168)
  │ │         💬 Args: [no args]
  │ │         👁️  Def: private
  │ └─ [2] 🔒 MODIFIER: Initializable.initializer() (NodeID: 169)
  │     💬 Args: [no args]
  │   └─ [3] ⚙️ FUNCTION: Initializable._getInitializableStorage() (NodeID: 170)
  │       💬 Args: [no args]
  │       👁️  Def: private
  │     └─ [4] ⚙️ FUNCTION: Initializable._initializableStorageSlot() (NodeID: 171)
  │         💬 Args: [no args]
  │         👁️  Def: internal
  └─ [1] 🔒 MODIFIER: Initializable.initializer() (NodeID: 172)
      💬 Args: [no args]
    └─ [2] ⚙️ FUNCTION: Initializable._getInitializableStorage() (NodeID: 173)
        💬 Args: [no args]
        👁️  Def: private
      └─ [3] ⚙️ FUNCTION: Initializable._initializableStorageSlot() (NodeID: 174)
          💬 Args: [no args]
          👁️  Def: internal
```

## Documentation

### Function Documentation

@notice Initializes the VeryLiquidVault with strategies
 @param auth_ The address of the Auth contract
 @param asset_ The address of the asset
 @param name_ The name of the vault
 @param symbol_ The symbol of the vault
 @param fundingAccount The address of the funding account for the first deposit, which will be treated as dead shares
 @param firstDepositAmount The amount of the first deposit, which will be treated as dead shares
 @param strategies_ The initial strategies to add to the vault
