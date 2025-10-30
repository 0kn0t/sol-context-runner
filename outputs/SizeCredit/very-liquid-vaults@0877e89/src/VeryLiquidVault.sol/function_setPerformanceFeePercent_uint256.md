# Function: setPerformanceFeePercent(uint256)

**Contract**: [src/VeryLiquidVault.sol/contract_VeryLiquidVault.md]

## Metadata

- **Contract**: VeryLiquidVault
- **Signature**: `setPerformanceFeePercent(uint256)`
- **Visibility**: external
- **Source Range**: 10872:211:145

## Implementation

```solidity
/// @notice Sets the performance fee percent
///  @param performanceFeePercent_ The new performance fee percent
///  @dev Only callable by addresses with DEFAULT_ADMIN_ROLE
function setPerformanceFeePercent(uint256 performanceFeePercent_) external nonReentrant() onlyAuth(DEFAULT_ADMIN_ROLE) {
    _setPerformanceFeePercent(performanceFeePercent_);
}
```

## Related Implementations

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

### onlyAuth(bytes32)

- **Kind**: modifier
- **Source**: 4783:80:149
- **Link**: `src/utils/BaseVault.sol:BaseVault:onlyAuth(bytes32)`

```solidity
/// @notice Modifier to restrict function access to addresses with specific roles
///  @dev Reverts if the caller doesn't have the required role
modifier onlyAuth(bytes32 role) {
    _checkAuthRole(role);
    _;
}
```

### _checkAuthRole(bytes32)

- **Kind**: internal
- **Source**: 5663:208:149
- **Link**: `src/utils/BaseVault.sol:BaseVault:_checkAuthRole(bytes32)`

```solidity
/// @notice Checks that the caller has the required role
///  @dev Reverts if the caller doesn't have the required role
function _checkAuthRole(bytes32 role) internal view {
    if (!auth().hasRole(role, _msgSender())) {
        revert IAccessControl.AccessControlUnauthorizedAccount(_msgSender(), role);
    }
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

### _msgSender()

- **Kind**: internal
- **Source**: 887:96:61
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/utils/ContextUpgradeable.sol:ContextUpgradeable:_msgSender()`

```solidity
function _msgSender() virtual internal view returns (address) {
    return msg.sender;
}
```

### nonReentrant()

- **Kind**: modifier
- **Source**: 3361:103:65
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
- **Source**: 3470:384:65
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

### _nonReentrantAfter()

- **Kind**: internal
- **Source**: 3860:283:65
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/utils/ReentrancyGuardUpgradeable.sol:ReentrancyGuardUpgradeable:_nonReentrantAfter()`

```solidity
function _nonReentrantAfter() private {
    ReentrancyGuardStorage storage $ = _getReentrancyGuardStorage();
    $._status = NOT_ENTERED;
}
```

## State Variable Reads

- **MAXIMUM_PERFORMANCE_FEE_PERCENT** (`uint256`)
- **ENTERED** (`uint256`)
- **NOT_ENTERED** (`uint256`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: VeryLiquidVault.setPerformanceFeePercent(uint256) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: external
  ├─ [1] ⚙️ FUNCTION: PerformanceVault._setPerformanceFeePercent(uint256) (NodeID: 1)
  │   💬 Args: [performanceFeePercent_]
  │   👁️  Def: internal
  │ ├─ [2] ⚙️ FUNCTION: PerformanceVault._getPerformanceVaultStorage() (NodeID: 2)
  │ │   💬 Args: [no args]
  │ │   👁️  Def: private
  │ ├─ [2] ⚙️ FUNCTION: PerformanceVault._pps() (NodeID: 3)
  │ │   💬 Args: [no args]
  │ │   👁️  Def: private
  │ │ ├─ [3] ⚙️ FUNCTION: VeryLiquidVault.totalAssets() (NodeID: 4)
  │ │ │   💬 Args: [no args]
  │ │ │   👁️  Def: public
  │ │ │ └─ [4] ⚙️ FUNCTION: VeryLiquidVault._getVeryLiquidVaultStorage() (NodeID: 5)
  │ │ │     💬 Args: [no args]
  │ │ │     👁️  Def: private
  │ │ ├─ [3] ⚙️ FUNCTION: ERC20Upgradeable.totalSupply() (NodeID: 6)
  │ │ │   💬 Args: [no args]
  │ │ │   👁️  Def: public
  │ │ │ └─ [4] ⚙️ FUNCTION: ERC20Upgradeable._getERC20Storage() (NodeID: 7)
  │ │ │     💬 Args: [no args]
  │ │ │     👁️  Def: private
  │ │ └─ [3] ⚙️ FUNCTION: Math.mulDiv(uint256,uint256,uint256) (NodeID: 8)
  │ │     💬 Args: [totalAssets_, PERCENT, totalSupply_]
  │ │     👁️  Def: internal
  │ │   ├─ [4] ⚙️ FUNCTION: Math.mul512(uint256,uint256) (NodeID: 9)
  │ │   │   💬 Args: [x, y]
  │ │   │   👁️  Def: internal
  │ │   └─ [4] ⚙️ FUNCTION: Panic.panic(uint256) (NodeID: 10)
  │ │       💬 Args: [ternary(denominator == 0, Panic.DIVISION_BY_ZERO, Panic.UNDER_OVERFLOW)]
  │ │       👁️  Def: internal
  │ │     └─ [5] ⚙️ FUNCTION: Math.ternary(bool,uint256,uint256) (NodeID: 11)
  │ │         💬 Args: [denominator == 0, Panic.DIVISION_BY_ZERO, Panic.UNDER_OVERFLOW]
  │ │         👁️  Def: internal
  │ │       └─ [6] ⚙️ FUNCTION: SafeCast.toUint(bool) (NodeID: 12)
  │ │           💬 Args: [condition]
  │ │           👁️  Def: internal
  │ └─ [2] ⚙️ FUNCTION: PerformanceVault._setHighWaterMark(uint256) (NodeID: 13)
  │     💬 Args: [currentPPS]
  │     👁️  Def: internal
  │   └─ [3] ⚙️ FUNCTION: PerformanceVault._getPerformanceVaultStorage() (NodeID: 14)
  │       💬 Args: [no args]
  │       👁️  Def: private
  ├─ [1] 🔒 MODIFIER: BaseVault.onlyAuth(bytes32) (NodeID: 15)
  │   💬 Args: [DEFAULT_ADMIN_ROLE]
  │ └─ [2] ⚙️ FUNCTION: BaseVault._checkAuthRole(bytes32) (NodeID: 16)
  │     💬 Args: [DEFAULT_ADMIN_ROLE]
  │     👁️  Def: internal
  │   ├─ [3] ⚙️ FUNCTION: BaseVault.auth() (NodeID: 17)
  │   │   💬 Args: [no args]
  │   │   👁️  Def: public
  │   │ └─ [4] ⚙️ FUNCTION: BaseVault._getBaseVaultStorage() (NodeID: 18)
  │   │     💬 Args: [no args]
  │   │     👁️  Def: private
  │   ├─ [3] ⚙️ FUNCTION: ContextUpgradeable._msgSender() (NodeID: 19)
  │   │   💬 Args: [no args]
  │   │   👁️  Def: internal
  │   └─ [3] ⚙️ FUNCTION: ContextUpgradeable._msgSender() (NodeID: 20)
  │       💬 Args: [no args]
  │       👁️  Def: internal
  └─ [1] 🔒 MODIFIER: ReentrancyGuardUpgradeable.nonReentrant() (NodeID: 21)
      💬 Args: [no args]
    ├─ [2] ⚙️ FUNCTION: ReentrancyGuardUpgradeable._nonReentrantBefore() (NodeID: 22)
    │   💬 Args: [no args]
    │   👁️  Def: private
    │ └─ [3] ⚙️ FUNCTION: ReentrancyGuardUpgradeable._getReentrancyGuardStorage() (NodeID: 23)
    │     💬 Args: [no args]
    │     👁️  Def: private
    └─ [2] ⚙️ FUNCTION: ReentrancyGuardUpgradeable._nonReentrantAfter() (NodeID: 24)
        💬 Args: [no args]
        👁️  Def: private
      └─ [3] ⚙️ FUNCTION: ReentrancyGuardUpgradeable._getReentrancyGuardStorage() (NodeID: 25)
          💬 Args: [no args]
          👁️  Def: private
```

## Documentation

### Function Documentation

@notice Sets the performance fee percent
 @param performanceFeePercent_ The new performance fee percent
 @dev Only callable by addresses with DEFAULT_ADMIN_ROLE
