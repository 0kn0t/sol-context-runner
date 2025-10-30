# Function: initialize(contract Auth,contract IERC20,string,string,address,uint256)

**Contract**: [src/SizeMetaVault.sol/contract_SizeMetaVault.md]

## Metadata

- **Contract**: SizeMetaVault
- **Signature**: `initialize(contract Auth,contract IERC20,string,string,address,uint256)`
- **Visibility**: public
- **Source Range**: 3411:896:56
- **Inherited From**: BaseVault

## Implementation

```solidity
/// @notice Initializes the BaseVault with necessary parameters
///  @dev Sets up all inherited contracts and makes the first deposit to prevent inflation attacks
function initialize(Auth auth_, IERC20 asset_, string memory name_, string memory symbol_, address fundingAccount_, uint256 firstDepositAmount_) virtual public initializer() {
    __ERC4626_init(asset_);
    __ERC20_init(name_, symbol_);
    __ERC20Permit_init(name_);
    __ReentrancyGuard_init();
    __Pausable_init();
    __Multicall_init();
    __UUPSUpgradeable_init();
    if (address(auth_) == address(0)) {
        revert NullAddress();
    }
    if (firstDepositAmount_ == 0) {
        revert NullAmount();
    }
    auth = auth_;
    emit AuthSet(address(auth_));
    deadAssets = firstDepositAmount_;
    emit DeadAssetsSet(firstDepositAmount_);
    _setTotalAssetsCap(type(uint256).max);
    _firstDeposit(fundingAccount_, firstDepositAmount_);
}
```

## Related Implementations

### __ERC4626_init(contract IERC20)

- **Kind**: internal
- **Source**: 5095:114:17
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/token/ERC20/extensions/ERC4626Upgradeable.sol:ERC4626Upgradeable:__ERC4626_init(contract IERC20)`

```solidity
///  @dev Set the underlying asset contract. This must be an ERC20-compatible contract (ERC-20 or ERC-777).
function __ERC4626_init(IERC20 asset_) internal onlyInitializing() {
    __ERC4626_init_unchained(asset_);
}
```

### __ERC4626_init_unchained(contract IERC20)

- **Kind**: internal
- **Source**: 5215:304:17
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
- **Source**: 4088:159:17
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
- **Source**: 5662:550:17
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
- **Source**: 6891:76:13
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
- **Source**: 7082:141:13
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
- **Source**: 8485:120:13
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/proxy/utils/Initializable.sol:Initializable:_isInitializing()`

```solidity
///  @dev Returns `true` if the contract is currently initializing. See {onlyInitializing}.
function _isInitializing() internal view returns (bool) {
    return _getInitializableStorage()._initializing;
}
```

### _getInitializableStorage()

- **Kind**: internal
- **Source**: 9071:205:13
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
- **Source**: 8819:122:13
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
- **Source**: 2263:147:15
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
- **Source**: 2416:216:15
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
- **Source**: 1947:153:15
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
- **Source**: 1832:125:16
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
- **Source**: 3599:330:23
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
- **Source**: 2720:156:23
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
- **Source**: 2684:111:22
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/utils/ReentrancyGuardUpgradeable.sol:ReentrancyGuardUpgradeable:__ReentrancyGuard_init()`

```solidity
function __ReentrancyGuard_init() internal onlyInitializing() {
    __ReentrancyGuard_init_unchained();
}
```

### __ReentrancyGuard_init_unchained()

- **Kind**: internal
- **Source**: 2801:183:22
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/utils/ReentrancyGuardUpgradeable.sol:ReentrancyGuardUpgradeable:__ReentrancyGuard_init_unchained()`

```solidity
function __ReentrancyGuard_init_unchained() internal onlyInitializing() {
    ReentrancyGuardStorage storage $ = _getReentrancyGuardStorage();
    $._status = NOT_ENTERED;
}
```

### _getReentrancyGuardStorage()

- **Kind**: internal
- **Source**: 2395:183:22
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
- **Source**: 2266:60:21
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/utils/PausableUpgradeable.sol:PausableUpgradeable:__Pausable_init()`

```solidity
function __Pausable_init() internal onlyInitializing() {}
```

### __Multicall_init()

- **Kind**: internal
- **Source**: 1218:61:19
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/utils/MulticallUpgradeable.sol:MulticallUpgradeable:__Multicall_init()`

```solidity
function __Multicall_init() internal onlyInitializing() {}
```

### __UUPSUpgradeable_init()

- **Kind**: internal
- **Source**: 2970:67:14
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/proxy/utils/UUPSUpgradeable.sol:UUPSUpgradeable:__UUPSUpgradeable_init()`

```solidity
function __UUPSUpgradeable_init() internal onlyInitializing() {}
```

### _setTotalAssetsCap(uint256)

- **Kind**: internal
- **Source**: 6255:230:56
- **Link**: `src/BaseVault.sol:BaseVault:_setTotalAssetsCap(uint256)`

```solidity
/// @notice Sets the maximum total assets of the vault
function _setTotalAssetsCap(uint256 totalAssetsCap_) private {
    uint256 oldTotalAssetsCap = totalAssetsCap;
    totalAssetsCap = totalAssetsCap_;
    emit TotalAssetsCapSet(oldTotalAssetsCap, totalAssetsCap_);
}
```

### _firstDeposit(address,uint256)

- **Kind**: internal
- **Source**: 6708:466:56
- **Link**: `src/BaseVault.sol:BaseVault:_firstDeposit(address,uint256)`

```solidity
/// @notice This function is used to deposit the first amount of assets into the vault
///  @dev This is equivalent to deposit(firstDepositAmount_, address(this)); with _msgSender() replaced by fundingAccount_
function _firstDeposit(address fundingAccount_, uint256 firstDepositAmount_) private {
    address receiver = address(this);
    uint256 maxAssets = maxDeposit(receiver);
    if (firstDepositAmount_ > maxAssets) {
        revert ERC4626ExceededMaxDeposit(receiver, firstDepositAmount_, maxAssets);
    }
    uint256 shares = previewDeposit(firstDepositAmount_);
    _deposit(fundingAccount_, receiver, firstDepositAmount_, shares);
}
```

### maxDeposit(address)

- **Kind**: internal
- **Source**: 3979:163:59
- **Link**: `src/SizeMetaVault.sol:SizeMetaVault:maxDeposit(address)`

```solidity
/// @notice Returns the maximum amount that can be deposited
function maxDeposit(address receiver) override(BaseVault) public view returns (uint256) {
    return Math.min(_maxDeposit(), super.maxDeposit(receiver));
}
```

### min(uint256,uint256)

- **Kind**: internal
- **Source**: 5617:111:52
- **Link**: `lib/openzeppelin-contracts/contracts/utils/math/Math.sol:Math:min(uint256,uint256)`

```solidity
///  @dev Returns the smallest of two numbers.
function min(uint256 a, uint256 b) internal pure returns (uint256) {
    return ternary(a < b, a, b);
}
```

### _maxDeposit()

- **Kind**: internal
- **Source**: 15537:355:59
- **Link**: `src/SizeMetaVault.sol:SizeMetaVault:_maxDeposit()`

```solidity
/// @notice Internal function to calculate maximum depositable amount in all strategies
function _maxDeposit() private view returns (uint256 max) {
    uint256 length = strategies.length;
    for (uint256 i = 0; i < length; i++) {
        IBaseVault strategy = strategies[i];
        uint256 strategyMaxDeposit = strategy.maxDeposit(address(this));
        max = Math.saturatingAdd(max, strategyMaxDeposit);
    }
}
```

### saturatingAdd(uint256,uint256)

- **Kind**: internal
- **Source**: 3912:199:52
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
- **Source**: 1693:240:52
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

### toUint(bool)

- **Kind**: internal
- **Source**: 34795:145:53
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
- **Source**: 5071:294:52
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

### maxDeposit(address)

- **Kind**: internal
- **Source**: 8184:249:56
- **Link**: `src/BaseVault.sol:BaseVault:maxDeposit(address)`

```solidity
/// @notice Returns the maximum amount that can be deposited
///  @dev Returns type(uint256).max if no total assets cap is set
function maxDeposit(address) virtual override(ERC4626Upgradeable, IERC4626) public view returns (uint256) {
    return (totalAssetsCap == type(uint256).max) ? type(uint256).max : Math.saturatingSub(totalAssetsCap, totalAssets());
}
```

### saturatingSub(uint256,uint256)

- **Kind**: internal
- **Source**: 4217:150:52
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
- **Source**: 5471:264:59
- **Link**: `src/SizeMetaVault.sol:SizeMetaVault:totalAssets()`

```solidity
/// @notice Returns the total assets managed by the vault
function totalAssets() virtual override(ERC4626Upgradeable, IERC4626) public view returns (uint256 total) {
    uint256 length = strategies.length;
    for (uint256 i = 0; i < length; i++) {
        total += strategies[i].totalAssets();
    }
}
```

### trySub(uint256,uint256)

- **Kind**: internal
- **Source**: 2052:240:52
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
- **Source**: 8338:147:17
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/token/ERC20/extensions/ERC4626Upgradeable.sol:ERC4626Upgradeable:previewDeposit(uint256)`

```solidity
/// @dev See {IERC4626-previewDeposit}. 
function previewDeposit(uint256 assets) virtual public view returns (uint256) {
    return _convertToShares(assets, Math.Rounding.Floor);
}
```

### _convertToShares(uint256,enum Math.Rounding)

- **Kind**: internal
- **Source**: 10972:213:17
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/token/ERC20/extensions/ERC4626Upgradeable.sol:ERC4626Upgradeable:_convertToShares(uint256,enum Math.Rounding)`

```solidity
///  @dev Internal conversion function (from assets to shares) with support for rounding direction.
function _convertToShares(uint256 assets, Math.Rounding rounding) virtual internal view returns (uint256) {
    return assets.mulDiv(totalSupply() + (10 ** _decimalsOffset()), totalAssets() + 1, rounding);
}
```

### mulDiv(uint256,uint256,uint256,enum Math.Rounding)

- **Kind**: internal
- **Source**: 11054:238:52
- **Link**: `lib/openzeppelin-contracts/contracts/utils/math/Math.sol:Math:mulDiv(uint256,uint256,uint256,enum Math.Rounding)`

```solidity
///  @dev Calculates x * y / denominator with full precision, following the selected rounding direction.
function mulDiv(uint256 x, uint256 y, uint256 denominator, Rounding rounding) internal pure returns (uint256) {
    return mulDiv(x, y, denominator) + SafeCast.toUint(unsignedRoundsUp(rounding) && (mulmod(x, y, denominator) > 0));
}
```

### _decimalsOffset()

- **Kind**: internal
- **Source**: 13425:90:17
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/token/ERC20/extensions/ERC4626Upgradeable.sol:ERC4626Upgradeable:_decimalsOffset()`

```solidity
function _decimalsOffset() virtual internal view returns (uint8) {
    return 0;
}
```

### totalSupply()

- **Kind**: internal
- **Source**: 3877:152:15
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
- **Source**: 32020:122:52
- **Link**: `lib/openzeppelin-contracts/contracts/utils/math/Math.sol:Math:unsignedRoundsUp(enum Math.Rounding)`

```solidity
///  @dev Returns whether a provided rounding mode is considered rounding up for unsigned integers.
function unsignedRoundsUp(Rounding rounding) internal pure returns (bool) {
    return (uint8(rounding) % 2) == 1;
}
```

### mulDiv(uint256,uint256,uint256)

- **Kind**: internal
- **Source**: 7242:3683:52
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
- **Source**: 1027:550:52
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
- **Source**: 1776:194:45
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
- **Source**: 5897:316:59
- **Link**: `src/SizeMetaVault.sol:SizeMetaVault:_deposit(address,address,uint256,uint256)`

```solidity
/// @notice Deposits assets to strategies in order
///  @dev Tries to deposit to strategies sequentially, reverts if not all assets can be deposited
function _deposit(address caller, address receiver, uint256 assets, uint256 shares) override internal {
    if (_isInitializing()) {
        shares = assets;
    }
    super._deposit(caller, receiver, assets, shares);
    _depositToStrategies(assets, shares);
}
```

### _deposit(address,address,uint256,uint256)

- **Kind**: internal
- **Source**: 11586:841:17
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
- **Source**: 1618:188:40
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
- **Source**: 6882:153:17
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
- **Source**: 8370:720:40
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
- **Source**: 8714:208:15
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
- **Source**: 3965:976:58
- **Link**: `src/PerformanceVault.sol:PerformanceVault:_update(address,address,uint256)`

```solidity
/// @notice Updates the high water mark and mints performance fees if applicable
function _update(address from, address to, uint256 value) override internal {
    super._update(from, to, value);
    if (performanceFeePercent == 0) {
        return;
    }
    uint256 currentPPS = Math.mulDiv(totalAssets(), PERCENT, totalSupply());
    uint256 highWaterMarkBefore = highWaterMark;
    if (currentPPS > highWaterMarkBefore) {
        uint256 profitPerSharePercent = currentPPS - highWaterMarkBefore;
        uint256 totalProfitShares = Math.mulDiv(profitPerSharePercent, totalSupply(), PERCENT);
        uint256 feeShares = Math.mulDiv(totalProfitShares, performanceFeePercent, PERCENT);
        if (feeShares > 0) {
            highWaterMark = currentPPS;
            emit HighWaterMarkUpdated(highWaterMarkBefore, currentPPS);
            _mint(feeRecipient, feeShares);
            emit PerformanceFeeMinted(feeRecipient, feeShares, convertToAssets(feeShares));
        }
    }
}
```

### _update(address,address,uint256)

- **Kind**: internal
- **Source**: 7817:227:56
- **Link**: `src/BaseVault.sol:BaseVault:_update(address,address,uint256)`

```solidity
/// @notice Internal function called during token transfers
///  @dev Ensures transfers only happen when the contract is not paused and that no reentrancy is possible
function _update(address from, address to, uint256 value) virtual override internal nonReentrant() notPaused() {
    super._update(from, to, value);
    emit VaultStatus(block.timestamp, totalSupply(), totalAssets());
}
```

### _update(address,address,uint256)

- **Kind**: internal
- **Source**: 7201:1170:15
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

### notPaused()

- **Kind**: modifier
- **Source**: 4981:102:56
- **Link**: `src/BaseVault.sol:BaseVault:notPaused()`

```solidity
/// @notice Modifier to ensure the contract is not paused
///  @dev Checks both local pause state and global pause state from Auth
modifier notPaused() {
    if (paused() || auth.paused()) revert EnforcedPause();
    _;
}
```

### paused()

- **Kind**: internal
- **Source**: 2496:145:21
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
- **Source**: 1147:162:21
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/utils/PausableUpgradeable.sol:PausableUpgradeable:_getPausableStorage()`

```solidity
function _getPausableStorage() private pure returns (PausableStorage storage $) {
    assembly {
        $.slot := PausableStorageLocation
    }
}
```

### nonReentrant()

- **Kind**: modifier
- **Source**: 3361:103:22
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/utils/ReentrancyGuardUpgradeable.sol:ReentrancyGuardUpgradeable:nonReentrant()`

```solidity
///  @dev Prevents a contract from calling itself, directly or indirectly.
///  Calling a `nonReentrant` function from another `nonReentrant`
///  function is not supported. It is possible to prevent this from happening
///  by making the `nonReentrant` function external, and making it call a
///  `private` function that does the actual work.
modifier nonReentrant() {
    _nonReentrantBefore();
    _;
    _nonReentrantAfter();
}
```

### _nonReentrantBefore()

- **Kind**: internal
- **Source**: 3470:384:22
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/utils/ReentrancyGuardUpgradeable.sol:ReentrancyGuardUpgradeable:_nonReentrantBefore()`

```solidity
function _nonReentrantBefore() private {
    ReentrancyGuardStorage storage $ = _getReentrancyGuardStorage();
    if ($._status == ENTERED) {
        revert ReentrancyGuardReentrantCall();
    }
    $._status = ENTERED;
}
```

### _nonReentrantAfter()

- **Kind**: internal
- **Source**: 3860:283:22
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/utils/ReentrancyGuardUpgradeable.sol:ReentrancyGuardUpgradeable:_nonReentrantAfter()`

```solidity
function _nonReentrantAfter() private {
    ReentrancyGuardStorage storage $ = _getReentrancyGuardStorage();
    $._status = NOT_ENTERED;
}
```

### convertToAssets(uint256)

- **Kind**: internal
- **Source**: 7466:148:17
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/token/ERC20/extensions/ERC4626Upgradeable.sol:ERC4626Upgradeable:convertToAssets(uint256)`

```solidity
/// @dev See {IERC4626-convertToAssets}. 
function convertToAssets(uint256 shares) virtual public view returns (uint256) {
    return _convertToAssets(shares, Math.Rounding.Floor);
}
```

### _convertToAssets(uint256,enum Math.Rounding)

- **Kind**: internal
- **Source**: 11309:213:17
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/token/ERC20/extensions/ERC4626Upgradeable.sol:ERC4626Upgradeable:_convertToAssets(uint256,enum Math.Rounding)`

```solidity
///  @dev Internal conversion function (from shares to assets) with support for rounding direction.
function _convertToAssets(uint256 shares, Math.Rounding rounding) virtual internal view returns (uint256) {
    return shares.mulDiv(totalAssets() + 1, totalSupply() + (10 ** _decimalsOffset()), rounding);
}
```

### _depositToStrategies(uint256,uint256)

- **Kind**: internal
- **Source**: 16468:1041:59
- **Link**: `src/SizeMetaVault.sol:SizeMetaVault:_depositToStrategies(uint256,uint256)`

```solidity
/// @notice Internal function to deposit assets to strategies
function _depositToStrategies(uint256 assets, uint256 shares) private {
    uint256 assetsToDeposit = assets;
    uint256 length = strategies.length;
    for (uint256 i = 0; i < length; i++) {
        IBaseVault strategy = strategies[i];
        uint256 strategyMaxDeposit = strategy.maxDeposit(address(this));
        uint256 depositAmount = Math.min(assetsToDeposit, strategyMaxDeposit);
        if (depositAmount == 0) {
            break;
        }
        IERC20(asset()).forceApprove(address(strategy), depositAmount);
        try strategy.deposit(depositAmount, address(this)) {
            assetsToDeposit -= depositAmount;
        } catch {
            IERC20(asset()).forceApprove(address(strategy), 0);
        }
    }
    if (assetsToDeposit > 0) {
        revert CannotDepositToStrategies(assets, shares, assetsToDeposit);
    }
}
```

### initializer()

- **Kind**: modifier
- **Source**: 4069:1102:13
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

## State Variable Reads

- **INITIALIZABLE_STORAGE** (`bytes32`)
- **NOT_ENTERED** (`uint256`)
- **totalAssetsCap** (`uint256`)
- **strategies** (`contract IBaseVault[]`) [src/IBaseVault.sol/interface_IBaseVault.md]
- **performanceFeePercent** (`uint256`)
- **PERCENT** (`uint256`)
- **highWaterMark** (`uint256`)
- **feeRecipient** (`address`)
- **auth** (`contract Auth`) [src/utils/Auth.sol/contract_Auth.md]
- **ENTERED** (`uint256`)

## State Variable Writes

- **auth** (`contract Auth`) [src/utils/Auth.sol/contract_Auth.md]
- **deadAssets** (`uint256`)
- **totalAssetsCap** (`uint256`)
- **highWaterMark** (`uint256`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: BaseVault.initialize(contract Auth,contract IERC20,string,string,address,uint256) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
  ├─ [1] ⚙️ FUNCTION: ERC4626Upgradeable.__ERC4626_init(contract IERC20) (NodeID: 1)
  │   💬 Args: [asset_]
  │   👁️  Def: internal
  │ ├─ [2] ⚙️ FUNCTION: ERC4626Upgradeable.__ERC4626_init_unchained(contract IERC20) (NodeID: 2)
  │ │   💬 Args: [asset_]
  │ │   👁️  Def: internal
  │ │ ├─ [3] ⚙️ FUNCTION: ERC4626Upgradeable._getERC4626Storage() (NodeID: 3)
  │ │ │   💬 Args: [no args]
  │ │ │   👁️  Def: private
  │ │ ├─ [3] ⚙️ FUNCTION: ERC4626Upgradeable._tryGetAssetDecimals(contract IERC20) (NodeID: 4)
  │ │ │   💬 Args: [asset_]
  │ │ │   👁️  Def: private
  │ │ └─ [3] 🔒 MODIFIER: Initializable.onlyInitializing() (NodeID: 5)
  │ │     💬 Args: [no args]
  │ │   └─ [4] ⚙️ FUNCTION: Initializable._checkInitializing() (NodeID: 6)
  │ │       💬 Args: [no args]
  │ │       👁️  Def: internal
  │ │     └─ [5] ⚙️ FUNCTION: Initializable._isInitializing() (NodeID: 7)
  │ │         💬 Args: [no args]
  │ │         👁️  Def: internal
  │ │       └─ [6] ⚙️ FUNCTION: Initializable._getInitializableStorage() (NodeID: 8)
  │ │           💬 Args: [no args]
  │ │           👁️  Def: private
  │ │         └─ [7] ⚙️ FUNCTION: Initializable._initializableStorageSlot() (NodeID: 9)
  │ │             💬 Args: [no args]
  │ │             👁️  Def: internal
  │ └─ [2] 🔒 MODIFIER: Initializable.onlyInitializing() (NodeID: 10)
  │     💬 Args: [no args]
  │   └─ [3] ⚙️ FUNCTION: Initializable._checkInitializing() (NodeID: 11)
  │       💬 Args: [no args]
  │       👁️  Def: internal
  │     └─ [4] ⚙️ FUNCTION: Initializable._isInitializing() (NodeID: 12)
  │         💬 Args: [no args]
  │         👁️  Def: internal
  │       └─ [5] ⚙️ FUNCTION: Initializable._getInitializableStorage() (NodeID: 13)
  │           💬 Args: [no args]
  │           👁️  Def: private
  │         └─ [6] ⚙️ FUNCTION: Initializable._initializableStorageSlot() (NodeID: 14)
  │             💬 Args: [no args]
  │             👁️  Def: internal
  ├─ [1] ⚙️ FUNCTION: ERC20Upgradeable.__ERC20_init(string,string) (NodeID: 15)
  │   💬 Args: [name_, symbol_]
  │   👁️  Def: internal
  │ ├─ [2] ⚙️ FUNCTION: ERC20Upgradeable.__ERC20_init_unchained(string,string) (NodeID: 16)
  │ │   💬 Args: [name_, symbol_]
  │ │   👁️  Def: internal
  │ │ ├─ [3] ⚙️ FUNCTION: ERC20Upgradeable._getERC20Storage() (NodeID: 17)
  │ │ │   💬 Args: [no args]
  │ │ │   👁️  Def: private
  │ │ └─ [3] 🔒 MODIFIER: Initializable.onlyInitializing() (NodeID: 18)
  │ │     💬 Args: [no args]
  │ │   └─ [4] ⚙️ FUNCTION: Initializable._checkInitializing() (NodeID: 19)
  │ │       💬 Args: [no args]
  │ │       👁️  Def: internal
  │ │     └─ [5] ⚙️ FUNCTION: Initializable._isInitializing() (NodeID: 20)
  │ │         💬 Args: [no args]
  │ │         👁️  Def: internal
  │ │       └─ [6] ⚙️ FUNCTION: Initializable._getInitializableStorage() (NodeID: 21)
  │ │           💬 Args: [no args]
  │ │           👁️  Def: private
  │ │         └─ [7] ⚙️ FUNCTION: Initializable._initializableStorageSlot() (NodeID: 22)
  │ │             💬 Args: [no args]
  │ │             👁️  Def: internal
  │ └─ [2] 🔒 MODIFIER: Initializable.onlyInitializing() (NodeID: 23)
  │     💬 Args: [no args]
  │   └─ [3] ⚙️ FUNCTION: Initializable._checkInitializing() (NodeID: 24)
  │       💬 Args: [no args]
  │       👁️  Def: internal
  │     └─ [4] ⚙️ FUNCTION: Initializable._isInitializing() (NodeID: 25)
  │         💬 Args: [no args]
  │         👁️  Def: internal
  │       └─ [5] ⚙️ FUNCTION: Initializable._getInitializableStorage() (NodeID: 26)
  │           💬 Args: [no args]
  │           👁️  Def: private
  │         └─ [6] ⚙️ FUNCTION: Initializable._initializableStorageSlot() (NodeID: 27)
  │             💬 Args: [no args]
  │             👁️  Def: internal
  ├─ [1] ⚙️ FUNCTION: ERC20PermitUpgradeable.__ERC20Permit_init(string) (NodeID: 28)
  │   💬 Args: [name_]
  │   👁️  Def: internal
  │ ├─ [2] ⚙️ FUNCTION: EIP712Upgradeable.__EIP712_init_unchained(string,string) (NodeID: 29)
  │ │   💬 Args: [name, "1"]
  │ │   👁️  Def: internal
  │ │ ├─ [3] ⚙️ FUNCTION: EIP712Upgradeable._getEIP712Storage() (NodeID: 30)
  │ │ │   💬 Args: [no args]
  │ │ │   👁️  Def: private
  │ │ └─ [3] 🔒 MODIFIER: Initializable.onlyInitializing() (NodeID: 31)
  │ │     💬 Args: [no args]
  │ │   └─ [4] ⚙️ FUNCTION: Initializable._checkInitializing() (NodeID: 32)
  │ │       💬 Args: [no args]
  │ │       👁️  Def: internal
  │ │     └─ [5] ⚙️ FUNCTION: Initializable._isInitializing() (NodeID: 33)
  │ │         💬 Args: [no args]
  │ │         👁️  Def: internal
  │ │       └─ [6] ⚙️ FUNCTION: Initializable._getInitializableStorage() (NodeID: 34)
  │ │           💬 Args: [no args]
  │ │           👁️  Def: private
  │ │         └─ [7] ⚙️ FUNCTION: Initializable._initializableStorageSlot() (NodeID: 35)
  │ │             💬 Args: [no args]
  │ │             👁️  Def: internal
  │ └─ [2] 🔒 MODIFIER: Initializable.onlyInitializing() (NodeID: 36)
  │     💬 Args: [no args]
  │   └─ [3] ⚙️ FUNCTION: Initializable._checkInitializing() (NodeID: 37)
  │       💬 Args: [no args]
  │       👁️  Def: internal
  │     └─ [4] ⚙️ FUNCTION: Initializable._isInitializing() (NodeID: 38)
  │         💬 Args: [no args]
  │         👁️  Def: internal
  │       └─ [5] ⚙️ FUNCTION: Initializable._getInitializableStorage() (NodeID: 39)
  │           💬 Args: [no args]
  │           👁️  Def: private
  │         └─ [6] ⚙️ FUNCTION: Initializable._initializableStorageSlot() (NodeID: 40)
  │             💬 Args: [no args]
  │             👁️  Def: internal
  ├─ [1] ⚙️ FUNCTION: ReentrancyGuardUpgradeable.__ReentrancyGuard_init() (NodeID: 41)
  │   💬 Args: [no args]
  │   👁️  Def: internal
  │ ├─ [2] ⚙️ FUNCTION: ReentrancyGuardUpgradeable.__ReentrancyGuard_init_unchained() (NodeID: 42)
  │ │   💬 Args: [no args]
  │ │   👁️  Def: internal
  │ │ ├─ [3] ⚙️ FUNCTION: ReentrancyGuardUpgradeable._getReentrancyGuardStorage() (NodeID: 43)
  │ │ │   💬 Args: [no args]
  │ │ │   👁️  Def: private
  │ │ └─ [3] 🔒 MODIFIER: Initializable.onlyInitializing() (NodeID: 44)
  │ │     💬 Args: [no args]
  │ │   └─ [4] ⚙️ FUNCTION: Initializable._checkInitializing() (NodeID: 45)
  │ │       💬 Args: [no args]
  │ │       👁️  Def: internal
  │ │     └─ [5] ⚙️ FUNCTION: Initializable._isInitializing() (NodeID: 46)
  │ │         💬 Args: [no args]
  │ │         👁️  Def: internal
  │ │       └─ [6] ⚙️ FUNCTION: Initializable._getInitializableStorage() (NodeID: 47)
  │ │           💬 Args: [no args]
  │ │           👁️  Def: private
  │ │         └─ [7] ⚙️ FUNCTION: Initializable._initializableStorageSlot() (NodeID: 48)
  │ │             💬 Args: [no args]
  │ │             👁️  Def: internal
  │ └─ [2] 🔒 MODIFIER: Initializable.onlyInitializing() (NodeID: 49)
  │     💬 Args: [no args]
  │   └─ [3] ⚙️ FUNCTION: Initializable._checkInitializing() (NodeID: 50)
  │       💬 Args: [no args]
  │       👁️  Def: internal
  │     └─ [4] ⚙️ FUNCTION: Initializable._isInitializing() (NodeID: 51)
  │         💬 Args: [no args]
  │         👁️  Def: internal
  │       └─ [5] ⚙️ FUNCTION: Initializable._getInitializableStorage() (NodeID: 52)
  │           💬 Args: [no args]
  │           👁️  Def: private
  │         └─ [6] ⚙️ FUNCTION: Initializable._initializableStorageSlot() (NodeID: 53)
  │             💬 Args: [no args]
  │             👁️  Def: internal
  ├─ [1] ⚙️ FUNCTION: PausableUpgradeable.__Pausable_init() (NodeID: 54)
  │   💬 Args: [no args]
  │   👁️  Def: internal
  │ └─ [2] 🔒 MODIFIER: Initializable.onlyInitializing() (NodeID: 55)
  │     💬 Args: [no args]
  │   └─ [3] ⚙️ FUNCTION: Initializable._checkInitializing() (NodeID: 56)
  │       💬 Args: [no args]
  │       👁️  Def: internal
  │     └─ [4] ⚙️ FUNCTION: Initializable._isInitializing() (NodeID: 57)
  │         💬 Args: [no args]
  │         👁️  Def: internal
  │       └─ [5] ⚙️ FUNCTION: Initializable._getInitializableStorage() (NodeID: 58)
  │           💬 Args: [no args]
  │           👁️  Def: private
  │         └─ [6] ⚙️ FUNCTION: Initializable._initializableStorageSlot() (NodeID: 59)
  │             💬 Args: [no args]
  │             👁️  Def: internal
  ├─ [1] ⚙️ FUNCTION: MulticallUpgradeable.__Multicall_init() (NodeID: 60)
  │   💬 Args: [no args]
  │   👁️  Def: internal
  │ └─ [2] 🔒 MODIFIER: Initializable.onlyInitializing() (NodeID: 61)
  │     💬 Args: [no args]
  │   └─ [3] ⚙️ FUNCTION: Initializable._checkInitializing() (NodeID: 62)
  │       💬 Args: [no args]
  │       👁️  Def: internal
  │     └─ [4] ⚙️ FUNCTION: Initializable._isInitializing() (NodeID: 63)
  │         💬 Args: [no args]
  │         👁️  Def: internal
  │       └─ [5] ⚙️ FUNCTION: Initializable._getInitializableStorage() (NodeID: 64)
  │           💬 Args: [no args]
  │           👁️  Def: private
  │         └─ [6] ⚙️ FUNCTION: Initializable._initializableStorageSlot() (NodeID: 65)
  │             💬 Args: [no args]
  │             👁️  Def: internal
  ├─ [1] ⚙️ FUNCTION: UUPSUpgradeable.__UUPSUpgradeable_init() (NodeID: 66)
  │   💬 Args: [no args]
  │   👁️  Def: internal
  │ └─ [2] 🔒 MODIFIER: Initializable.onlyInitializing() (NodeID: 67)
  │     💬 Args: [no args]
  │   └─ [3] ⚙️ FUNCTION: Initializable._checkInitializing() (NodeID: 68)
  │       💬 Args: [no args]
  │       👁️  Def: internal
  │     └─ [4] ⚙️ FUNCTION: Initializable._isInitializing() (NodeID: 69)
  │         💬 Args: [no args]
  │         👁️  Def: internal
  │       └─ [5] ⚙️ FUNCTION: Initializable._getInitializableStorage() (NodeID: 70)
  │           💬 Args: [no args]
  │           👁️  Def: private
  │         └─ [6] ⚙️ FUNCTION: Initializable._initializableStorageSlot() (NodeID: 71)
  │             💬 Args: [no args]
  │             👁️  Def: internal
  ├─ [1] ⚙️ FUNCTION: BaseVault._setTotalAssetsCap(uint256) (NodeID: 72)
  │   💬 Args: [type(uint256).max]
  │   👁️  Def: private
  ├─ [1] ⚙️ FUNCTION: BaseVault._firstDeposit(address,uint256) (NodeID: 73)
  │   💬 Args: [fundingAccount_, firstDepositAmount_]
  │   👁️  Def: private
  │ ├─ [2] ⚙️ FUNCTION: SizeMetaVault.maxDeposit(address) (NodeID: 74)
  │ │   💬 Args: [receiver]
  │ │   👁️  Def: public
  │ │ └─ [3] ⚙️ FUNCTION: Math.min(uint256,uint256) (NodeID: 75)
  │ │     💬 Args: [_maxDeposit(), super.maxDeposit(receiver)]
  │ │     👁️  Def: internal
  │ │   ├─ [4] ⚙️ FUNCTION: SizeMetaVault._maxDeposit() (NodeID: 78)
  │ │   │   💬 Args: [no args]
  │ │   │   👁️  Def: private
  │ │   │ └─ [5] ⚙️ FUNCTION: Math.saturatingAdd(uint256,uint256) (NodeID: 79)
  │ │   │     💬 Args: [max, strategyMaxDeposit]
  │ │   │     👁️  Def: internal
  │ │   │   ├─ [6] ⚙️ FUNCTION: Math.tryAdd(uint256,uint256) (NodeID: 80)
  │ │   │   │   💬 Args: [a, b]
  │ │   │   │   👁️  Def: internal
  │ │   │   │ └─ [7] ⚙️ FUNCTION: SafeCast.toUint(bool) (NodeID: 81)
  │ │   │   │     💬 Args: [success]
  │ │   │   │     👁️  Def: internal
  │ │   │   └─ [6] ⚙️ FUNCTION: Math.ternary(bool,uint256,uint256) (NodeID: 82)
  │ │   │       💬 Args: [success, result, type(uint256).max]
  │ │   │       👁️  Def: internal
  │ │   │     └─ [7] ⚙️ FUNCTION: SafeCast.toUint(bool) (NodeID: 83)
  │ │   │         💬 Args: [condition]
  │ │   │         👁️  Def: internal
  │ │   ├─ [4] ⚙️ FUNCTION: BaseVault.maxDeposit(address) (NodeID: 84)
  │ │   │   💬 Args: [receiver]
  │ │   │   👁️  Def: public
  │ │   │ └─ [5] ⚙️ FUNCTION: Math.saturatingSub(uint256,uint256) (NodeID: 85)
  │ │   │     💬 Args: [totalAssetsCap, totalAssets()]
  │ │   │     👁️  Def: internal
  │ │   │   ├─ [6] ⚙️ FUNCTION: SizeMetaVault.totalAssets() (NodeID: 88)
  │ │   │   │   💬 Args: [no args]
  │ │   │   │   👁️  Def: public
  │ │   │   └─ [6] ⚙️ FUNCTION: Math.trySub(uint256,uint256) (NodeID: 86)
  │ │   │       💬 Args: [a, b]
  │ │   │       👁️  Def: internal
  │ │   │     └─ [7] ⚙️ FUNCTION: SafeCast.toUint(bool) (NodeID: 87)
  │ │   │         💬 Args: [success]
  │ │   │         👁️  Def: internal
  │ │   └─ [4] ⚙️ FUNCTION: Math.ternary(bool,uint256,uint256) (NodeID: 76)
  │ │       💬 Args: [a < b, a, b]
  │ │       👁️  Def: internal
  │ │     └─ [5] ⚙️ FUNCTION: SafeCast.toUint(bool) (NodeID: 77)
  │ │         💬 Args: [condition]
  │ │         👁️  Def: internal
  │ ├─ [2] ⚙️ FUNCTION: ERC4626Upgradeable.previewDeposit(uint256) (NodeID: 89)
  │ │   💬 Args: [firstDepositAmount_]
  │ │   👁️  Def: public
  │ │ └─ [3] ⚙️ FUNCTION: ERC4626Upgradeable._convertToShares(uint256,enum Math.Rounding) (NodeID: 90)
  │ │     💬 Args: [assets, Math.Rounding.Floor]
  │ │     👁️  Def: internal
  │ │   └─ [4] ⚙️ FUNCTION: Math.mulDiv(uint256,uint256,uint256,enum Math.Rounding) (NodeID: 91)
  │ │       💬 Args: [assets, totalSupply() + (10 ** _decimalsOffset()), totalAssets() + 1, rounding]
  │ │       👁️  Def: internal
  │ │     ├─ [5] ⚙️ FUNCTION: ERC4626Upgradeable._decimalsOffset() (NodeID: 99)
  │ │     │   💬 Args: [no args]
  │ │     │   👁️  Def: internal
  │ │     ├─ [5] ⚙️ FUNCTION: ERC20Upgradeable.totalSupply() (NodeID: 100)
  │ │     │   💬 Args: [no args]
  │ │     │   👁️  Def: public
  │ │     │ └─ [6] ⚙️ FUNCTION: ERC20Upgradeable._getERC20Storage() (NodeID: 101)
  │ │     │     💬 Args: [no args]
  │ │     │     👁️  Def: private
  │ │     ├─ [5] ⚙️ FUNCTION: SizeMetaVault.totalAssets() (NodeID: 102)
  │ │     │   💬 Args: [no args]
  │ │     │   👁️  Def: public
  │ │     ├─ [5] ⚙️ FUNCTION: SafeCast.toUint(bool) (NodeID: 92)
  │ │     │   💬 Args: [unsignedRoundsUp(rounding) && (mulmod(x, y, denominator) > 0)]
  │ │     │   👁️  Def: internal
  │ │     │ └─ [6] ⚙️ FUNCTION: Math.unsignedRoundsUp(enum Math.Rounding) (NodeID: 93)
  │ │     │     💬 Args: [rounding]
  │ │     │     👁️  Def: internal
  │ │     └─ [5] ⚙️ FUNCTION: Math.mulDiv(uint256,uint256,uint256) (NodeID: 94)
  │ │         💬 Args: [x, y, denominator]
  │ │         👁️  Def: internal
  │ │       ├─ [6] ⚙️ FUNCTION: Math.mul512(uint256,uint256) (NodeID: 95)
  │ │       │   💬 Args: [x, y]
  │ │       │   👁️  Def: internal
  │ │       └─ [6] ⚙️ FUNCTION: Panic.panic(uint256) (NodeID: 96)
  │ │           💬 Args: [ternary(denominator == 0, Panic.DIVISION_BY_ZERO, Panic.UNDER_OVERFLOW)]
  │ │           👁️  Def: internal
  │ │         └─ [7] ⚙️ FUNCTION: Math.ternary(bool,uint256,uint256) (NodeID: 97)
  │ │             💬 Args: [denominator == 0, Panic.DIVISION_BY_ZERO, Panic.UNDER_OVERFLOW]
  │ │             👁️  Def: internal
  │ │           └─ [8] ⚙️ FUNCTION: SafeCast.toUint(bool) (NodeID: 98)
  │ │               💬 Args: [condition]
  │ │               👁️  Def: internal
  │ └─ [2] ⚙️ FUNCTION: SizeMetaVault._deposit(address,address,uint256,uint256) (NodeID: 103)
  │     💬 Args: [fundingAccount_, receiver, firstDepositAmount_, shares]
  │     👁️  Def: internal
  │   ├─ [3] ⚙️ FUNCTION: Initializable._isInitializing() (NodeID: 104)
  │   │   💬 Args: [no args]
  │   │   👁️  Def: internal
  │   │ └─ [4] ⚙️ FUNCTION: Initializable._getInitializableStorage() (NodeID: 105)
  │   │     💬 Args: [no args]
  │   │     👁️  Def: private
  │   │   └─ [5] ⚙️ FUNCTION: Initializable._initializableStorageSlot() (NodeID: 106)
  │   │       💬 Args: [no args]
  │   │       👁️  Def: internal
  │   ├─ [3] ⚙️ FUNCTION: ERC4626Upgradeable._deposit(address,address,uint256,uint256) (NodeID: 107)
  │   │   💬 Args: [caller, receiver, assets, shares]
  │   │   👁️  Def: internal
  │   │ ├─ [4] ⚙️ FUNCTION: SafeERC20.safeTransferFrom(contract IERC20,address,address,uint256) (NodeID: 108)
  │   │ │   💬 Args: [IERC20(asset()), caller, address(this), assets]
  │   │ │   👁️  Def: internal
  │   │ │ ├─ [5] ⚙️ FUNCTION: ERC4626Upgradeable.asset() (NodeID: 110)
  │   │ │ │   💬 Args: [no args]
  │   │ │ │   👁️  Def: public
  │   │ │ │ └─ [6] ⚙️ FUNCTION: ERC4626Upgradeable._getERC4626Storage() (NodeID: 111)
  │   │ │ │     💬 Args: [no args]
  │   │ │ │     👁️  Def: private
  │   │ │ └─ [5] ⚙️ FUNCTION: SafeERC20._callOptionalReturn(contract IERC20,bytes) (NodeID: 109)
  │   │ │     💬 Args: [token, abi.encodeCall(token.transferFrom, (from, to, value))]
  │   │ │     👁️  Def: private
  │   │ └─ [4] ⚙️ FUNCTION: ERC20Upgradeable._mint(address,uint256) (NodeID: 112)
  │   │     💬 Args: [receiver, shares]
  │   │     👁️  Def: internal
  │   │   └─ [5] ⚙️ FUNCTION: PerformanceVault._update(address,address,uint256) (NodeID: 113)
  │   │       💬 Args: [address(0), account, value]
  │   │       👁️  Def: internal
  │   │     ├─ [6] ⚙️ FUNCTION: BaseVault._update(address,address,uint256) (NodeID: 114)
  │   │     │   💬 Args: [from, to, value]
  │   │     │   👁️  Def: internal
  │   │     │ ├─ [7] ⚙️ FUNCTION: ERC20Upgradeable._update(address,address,uint256) (NodeID: 115)
  │   │     │ │   💬 Args: [from, to, value]
  │   │     │ │   👁️  Def: internal
  │   │     │ │ └─ [8] ⚙️ FUNCTION: ERC20Upgradeable._getERC20Storage() (NodeID: 116)
  │   │     │ │     💬 Args: [no args]
  │   │     │ │     👁️  Def: private
  │   │     │ ├─ [7] ⚙️ FUNCTION: ERC20Upgradeable.totalSupply() (NodeID: 117)
  │   │     │ │   💬 Args: [no args]
  │   │     │ │   👁️  Def: public
  │   │     │ │ └─ [8] ⚙️ FUNCTION: ERC20Upgradeable._getERC20Storage() (NodeID: 118)
  │   │     │ │     💬 Args: [no args]
  │   │     │ │     👁️  Def: private
  │   │     │ ├─ [7] ⚙️ FUNCTION: SizeMetaVault.totalAssets() (NodeID: 119)
  │   │     │ │   💬 Args: [no args]
  │   │     │ │   👁️  Def: public
  │   │     │ ├─ [7] 🔒 MODIFIER: BaseVault.notPaused() (NodeID: 120)
  │   │     │ │   💬 Args: [no args]
  │   │     │ │ └─ [8] ⚙️ FUNCTION: PausableUpgradeable.paused() (NodeID: 121)
  │   │     │ │     💬 Args: [no args]
  │   │     │ │     👁️  Def: public
  │   │     │ │   └─ [9] ⚙️ FUNCTION: PausableUpgradeable._getPausableStorage() (NodeID: 122)
  │   │     │ │       💬 Args: [no args]
  │   │     │ │       👁️  Def: private
  │   │     │ └─ [7] 🔒 MODIFIER: ReentrancyGuardUpgradeable.nonReentrant() (NodeID: 123)
  │   │     │     💬 Args: [no args]
  │   │     │   ├─ [8] ⚙️ FUNCTION: ReentrancyGuardUpgradeable._nonReentrantBefore() (NodeID: 124)
  │   │     │   │   💬 Args: [no args]
  │   │     │   │   👁️  Def: private
  │   │     │   │ └─ [9] ⚙️ FUNCTION: ReentrancyGuardUpgradeable._getReentrancyGuardStorage() (NodeID: 125)
  │   │     │   │     💬 Args: [no args]
  │   │     │   │     👁️  Def: private
  │   │     │   └─ [8] ⚙️ FUNCTION: ReentrancyGuardUpgradeable._nonReentrantAfter() (NodeID: 126)
  │   │     │       💬 Args: [no args]
  │   │     │       👁️  Def: private
  │   │     │     └─ [9] ⚙️ FUNCTION: ReentrancyGuardUpgradeable._getReentrancyGuardStorage() (NodeID: 127)
  │   │     │         💬 Args: [no args]
  │   │     │         👁️  Def: private
  │   │     ├─ [6] ⚙️ FUNCTION: Math.mulDiv(uint256,uint256,uint256) (NodeID: 128)
  │   │     │   💬 Args: [totalAssets(), PERCENT, totalSupply()]
  │   │     │   👁️  Def: internal
  │   │     │ ├─ [7] ⚙️ FUNCTION: SizeMetaVault.totalAssets() (NodeID: 133)
  │   │     │ │   💬 Args: [no args]
  │   │     │ │   👁️  Def: public
  │   │     │ ├─ [7] ⚙️ FUNCTION: ERC20Upgradeable.totalSupply() (NodeID: 134)
  │   │     │ │   💬 Args: [no args]
  │   │     │ │   👁️  Def: public
  │   │     │ │ └─ [8] ⚙️ FUNCTION: ERC20Upgradeable._getERC20Storage() (NodeID: 135)
  │   │     │ │     💬 Args: [no args]
  │   │     │ │     👁️  Def: private
  │   │     │ ├─ [7] ⚙️ FUNCTION: Math.mul512(uint256,uint256) (NodeID: 129)
  │   │     │ │   💬 Args: [x, y]
  │   │     │ │   👁️  Def: internal
  │   │     │ └─ [7] ⚙️ FUNCTION: Panic.panic(uint256) (NodeID: 130)
  │   │     │     💬 Args: [ternary(denominator == 0, Panic.DIVISION_BY_ZERO, Panic.UNDER_OVERFLOW)]
  │   │     │     👁️  Def: internal
  │   │     │   └─ [8] ⚙️ FUNCTION: Math.ternary(bool,uint256,uint256) (NodeID: 131)
  │   │     │       💬 Args: [denominator == 0, Panic.DIVISION_BY_ZERO, Panic.UNDER_OVERFLOW]
  │   │     │       👁️  Def: internal
  │   │     │     └─ [9] ⚙️ FUNCTION: SafeCast.toUint(bool) (NodeID: 132)
  │   │     │         💬 Args: [condition]
  │   │     │         👁️  Def: internal
  │   │     ├─ [6] ⚙️ FUNCTION: Math.mulDiv(uint256,uint256,uint256) (NodeID: 136)
  │   │     │   💬 Args: [profitPerSharePercent, totalSupply(), PERCENT]
  │   │     │   👁️  Def: internal
  │   │     │ ├─ [7] ⚙️ FUNCTION: ERC20Upgradeable.totalSupply() (NodeID: 141)
  │   │     │ │   💬 Args: [no args]
  │   │     │ │   👁️  Def: public
  │   │     │ │ └─ [8] ⚙️ FUNCTION: ERC20Upgradeable._getERC20Storage() (NodeID: 142)
  │   │     │ │     💬 Args: [no args]
  │   │     │ │     👁️  Def: private
  │   │     │ ├─ [7] ⚙️ FUNCTION: Math.mul512(uint256,uint256) (NodeID: 137)
  │   │     │ │   💬 Args: [x, y]
  │   │     │ │   👁️  Def: internal
  │   │     │ └─ [7] ⚙️ FUNCTION: Panic.panic(uint256) (NodeID: 138)
  │   │     │     💬 Args: [ternary(denominator == 0, Panic.DIVISION_BY_ZERO, Panic.UNDER_OVERFLOW)]
  │   │     │     👁️  Def: internal
  │   │     │   └─ [8] ⚙️ FUNCTION: Math.ternary(bool,uint256,uint256) (NodeID: 139)
  │   │     │       💬 Args: [denominator == 0, Panic.DIVISION_BY_ZERO, Panic.UNDER_OVERFLOW]
  │   │     │       👁️  Def: internal
  │   │     │     └─ [9] ⚙️ FUNCTION: SafeCast.toUint(bool) (NodeID: 140)
  │   │     │         💬 Args: [condition]
  │   │     │         👁️  Def: internal
  │   │     ├─ [6] ⚙️ FUNCTION: Math.mulDiv(uint256,uint256,uint256) (NodeID: 143)
  │   │     │   💬 Args: [totalProfitShares, performanceFeePercent, PERCENT]
  │   │     │   👁️  Def: internal
  │   │     │ ├─ [7] ⚙️ FUNCTION: Math.mul512(uint256,uint256) (NodeID: 144)
  │   │     │ │   💬 Args: [x, y]
  │   │     │ │   👁️  Def: internal
  │   │     │ └─ [7] ⚙️ FUNCTION: Panic.panic(uint256) (NodeID: 145)
  │   │     │     💬 Args: [ternary(denominator == 0, Panic.DIVISION_BY_ZERO, Panic.UNDER_OVERFLOW)]
  │   │     │     👁️  Def: internal
  │   │     │   └─ [8] ⚙️ FUNCTION: Math.ternary(bool,uint256,uint256) (NodeID: 146)
  │   │     │       💬 Args: [denominator == 0, Panic.DIVISION_BY_ZERO, Panic.UNDER_OVERFLOW]
  │   │     │       👁️  Def: internal
  │   │     │     └─ [9] ⚙️ FUNCTION: SafeCast.toUint(bool) (NodeID: 147)
  │   │     │         💬 Args: [condition]
  │   │     │         👁️  Def: internal
  │   │     ├─ [6] ⚙️ FUNCTION: ERC20Upgradeable._mint(address,uint256) (NodeID: 148)
  │   │     │   💬 Args: [feeRecipient, feeShares]
  │   │     │   👁️  Def: internal
  │   │     └─ [6] ⚙️ FUNCTION: ERC4626Upgradeable.convertToAssets(uint256) (NodeID: 149)
  │   │         💬 Args: [feeShares]
  │   │         👁️  Def: public
  │   │       └─ [7] ⚙️ FUNCTION: ERC4626Upgradeable._convertToAssets(uint256,enum Math.Rounding) (NodeID: 150)
  │   │           💬 Args: [shares, Math.Rounding.Floor]
  │   │           👁️  Def: internal
  │   │         └─ [8] ⚙️ FUNCTION: Math.mulDiv(uint256,uint256,uint256,enum Math.Rounding) (NodeID: 151)
  │   │             💬 Args: [shares, totalAssets() + 1, totalSupply() + (10 ** _decimalsOffset()), rounding]
  │   │             👁️  Def: internal
  │   │           ├─ [9] ⚙️ FUNCTION: SizeMetaVault.totalAssets() (NodeID: 159)
  │   │           │   💬 Args: [no args]
  │   │           │   👁️  Def: public
  │   │           ├─ [9] ⚙️ FUNCTION: ERC4626Upgradeable._decimalsOffset() (NodeID: 160)
  │   │           │   💬 Args: [no args]
  │   │           │   👁️  Def: internal
  │   │           ├─ [9] ⚙️ FUNCTION: ERC20Upgradeable.totalSupply() (NodeID: 161)
  │   │           │   💬 Args: [no args]
  │   │           │   👁️  Def: public
  │   │           │ └─ [10] ⚙️ FUNCTION: ERC20Upgradeable._getERC20Storage() (NodeID: 162)
  │   │           │     💬 Args: [no args]
  │   │           │     👁️  Def: private
  │   │           ├─ [9] ⚙️ FUNCTION: SafeCast.toUint(bool) (NodeID: 152)
  │   │           │   💬 Args: [unsignedRoundsUp(rounding) && (mulmod(x, y, denominator) > 0)]
  │   │           │   👁️  Def: internal
  │   │           │ └─ [10] ⚙️ FUNCTION: Math.unsignedRoundsUp(enum Math.Rounding) (NodeID: 153)
  │   │           │     💬 Args: [rounding]
  │   │           │     👁️  Def: internal
  │   │           └─ [9] ⚙️ FUNCTION: Math.mulDiv(uint256,uint256,uint256) (NodeID: 154)
  │   │               💬 Args: [x, y, denominator]
  │   │               👁️  Def: internal
  │   │             ├─ [10] ⚙️ FUNCTION: Math.mul512(uint256,uint256) (NodeID: 155)
  │   │             │   💬 Args: [x, y]
  │   │             │   👁️  Def: internal
  │   │             └─ [10] ⚙️ FUNCTION: Panic.panic(uint256) (NodeID: 156)
  │   │                 💬 Args: [ternary(denominator == 0, Panic.DIVISION_BY_ZERO, Panic.UNDER_OVERFLOW)]
  │   │                 👁️  Def: internal
  │   │               └─ [11] ⚙️ FUNCTION: Math.ternary(bool,uint256,uint256) (NodeID: 157)
  │   │                   💬 Args: [denominator == 0, Panic.DIVISION_BY_ZERO, Panic.UNDER_OVERFLOW]
  │   │                   👁️  Def: internal
  │   │                 └─ [12] ⚙️ FUNCTION: SafeCast.toUint(bool) (NodeID: 158)
  │   │                     💬 Args: [condition]
  │   │                     👁️  Def: internal
  │   └─ [3] ⚙️ FUNCTION: SizeMetaVault._depositToStrategies(uint256,uint256) (NodeID: 163)
  │       💬 Args: [assets, shares]
  │       👁️  Def: private
  │     ├─ [4] ⚙️ FUNCTION: Math.min(uint256,uint256) (NodeID: 164)
  │     │   💬 Args: [assetsToDeposit, strategyMaxDeposit]
  │     │   👁️  Def: internal
  │     │ └─ [5] ⚙️ FUNCTION: Math.ternary(bool,uint256,uint256) (NodeID: 165)
  │     │     💬 Args: [a < b, a, b]
  │     │     👁️  Def: internal
  │     │   └─ [6] ⚙️ FUNCTION: SafeCast.toUint(bool) (NodeID: 166)
  │     │       💬 Args: [condition]
  │     │       👁️  Def: internal
  │     ├─ [4] ⚙️ FUNCTION: ERC4626Upgradeable.asset() (NodeID: 167)
  │     │   💬 Args: [no args]
  │     │   👁️  Def: public
  │     │ └─ [5] ⚙️ FUNCTION: ERC4626Upgradeable._getERC4626Storage() (NodeID: 168)
  │     │     💬 Args: [no args]
  │     │     👁️  Def: private
  │     └─ [4] ⚙️ FUNCTION: ERC4626Upgradeable.asset() (NodeID: 169)
  │         💬 Args: [no args]
  │         👁️  Def: public
  │       └─ [5] ⚙️ FUNCTION: ERC4626Upgradeable._getERC4626Storage() (NodeID: 170)
  │           💬 Args: [no args]
  │           👁️  Def: private
  └─ [1] 🔒 MODIFIER: Initializable.initializer() (NodeID: 171)
      💬 Args: [no args]
    └─ [2] ⚙️ FUNCTION: Initializable._getInitializableStorage() (NodeID: 172)
        💬 Args: [no args]
        👁️  Def: private
      └─ [3] ⚙️ FUNCTION: Initializable._initializableStorageSlot() (NodeID: 173)
          💬 Args: [no args]
          👁️  Def: internal
```

## Documentation

### Function Documentation

@notice Initializes the BaseVault with necessary parameters
 @dev Sets up all inherited contracts and makes the first deposit to prevent inflation attacks
