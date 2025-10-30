# Function: setPerformanceFeePercent(uint256)

**Contract**: [src/SizeMetaVault.sol/contract_SizeMetaVault.md]

## Metadata

- **Contract**: SizeMetaVault
- **Signature**: `setPerformanceFeePercent(uint256)`
- **Visibility**: external
- **Source Range**: 7297:268:59

## Implementation

```solidity
/// @notice Sets the performance fee percent
function setPerformanceFeePercent(uint256 performanceFeePercent_) external notPaused() onlyAuth(DEFAULT_ADMIN_ROLE) {
    if (_updateTimelockStateAndCheckIfTimelocked()) {
        return;
    }
    _setPerformanceFeePercent(performanceFeePercent_);
}
```

## Related Implementations

### _updateTimelockStateAndCheckIfTimelocked()

- **Kind**: internal
- **Source**: 2474:781:64
- **Link**: `src/utils/Timelock.sol:Timelock:_updateTimelockStateAndCheckIfTimelocked()`

```solidity
/// @notice Updates the timelock state and checks if the current function is timelocked
///  @return True if the current function is timelocked, false otherwise
function _updateTimelockStateAndCheckIfTimelocked() internal returns (bool) {
    bytes4 sig = msg.sig;
    bytes memory data = msg.data;
    if (timelockData[sig].proposedCalldataHash != keccak256(data)) {
        timelockData[sig].proposedTimestamp = block.timestamp;
        timelockData[sig].proposedCalldataHash = keccak256(data);
        emit ActionTimelocked(sig, data, _getTimelockDuration(sig) + timelockData[sig].proposedTimestamp);
    } else if (block.timestamp >= (_getTimelockDuration(sig) + timelockData[sig].proposedTimestamp)) {
        delete timelockData[sig].proposedTimestamp;
        delete timelockData[sig].proposedCalldataHash;
        emit TimelockExpired(sig);
    }
    return isTimelocked(msg.sig);
}
```

### _getTimelockDuration(bytes4)

- **Kind**: internal
- **Source**: 4424:161:64
- **Link**: `src/utils/Timelock.sol:Timelock:_getTimelockDuration(bytes4)`

```solidity
/// @notice Gets the timelock duration for a specific function
function _getTimelockDuration(bytes4 sig) internal view returns (uint256) {
    return Math.max(MINIMUM_TIMELOCK_DURATION, timelockData[sig].duration);
}
```

### max(uint256,uint256)

- **Kind**: internal
- **Source**: 5435:111:52
- **Link**: `lib/openzeppelin-contracts/contracts/utils/math/Math.sol:Math:max(uint256,uint256)`

```solidity
///  @dev Returns the largest of two numbers.
function max(uint256 a, uint256 b) internal pure returns (uint256) {
    return ternary(a > b, a, b);
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

### isTimelocked(bytes4)

- **Kind**: internal
- **Source**: 1945:166:64
- **Link**: `src/utils/Timelock.sol:Timelock:isTimelocked(bytes4)`

```solidity
/// @notice Checks if the function is timelocked
///  @return True if the function is timelocked, false otherwise
function isTimelocked(bytes4 sig) public view returns (bool) {
    return block.timestamp < (_getTimelockDuration(sig) + timelockData[sig].proposedTimestamp);
}
```

### _setPerformanceFeePercent(uint256)

- **Kind**: internal
- **Source**: 3121:489:58
- **Link**: `src/PerformanceVault.sol:PerformanceVault:_setPerformanceFeePercent(uint256)`

```solidity
/// @notice Sets the performance fee percent
///  @dev Reverts if the performance fee percent is greater than the maximum performance fee percent
function _setPerformanceFeePercent(uint256 performanceFeePercent_) internal {
    if (performanceFeePercent_ > MAXIMUM_PERFORMANCE_FEE_PERCENT) {
        revert PerformanceFeePercentTooHigh(performanceFeePercent_, MAXIMUM_PERFORMANCE_FEE_PERCENT);
    }
    uint256 performanceFeePercentBefore = performanceFeePercent;
    performanceFeePercent = performanceFeePercent_;
    emit PerformanceFeePercentSet(performanceFeePercentBefore, performanceFeePercent_);
}
```

### onlyAuth(bytes32)

- **Kind**: modifier
- **Source**: 4644:193:56
- **Link**: `src/BaseVault.sol:BaseVault:onlyAuth(bytes32)`

```solidity
/// @notice Modifier to restrict function access to addresses with specific roles
///  @dev Reverts if the caller doesn't have the required role
modifier onlyAuth(bytes32 role) {
    if (!auth.hasRole(role, msg.sender)) {
        revert IAccessControl.AccessControlUnauthorizedAccount(msg.sender, role);
    }
    _;
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

## State Variable Reads

- **timelockData** (`mapping(bytes4 => struct Timelock.TimelockData)`)
- **MINIMUM_TIMELOCK_DURATION** (`uint256`)
- **MAXIMUM_PERFORMANCE_FEE_PERCENT** (`uint256`)
- **performanceFeePercent** (`uint256`)
- **auth** (`contract Auth`) [src/utils/Auth.sol/contract_Auth.md]

## State Variable Writes

- **timelockData** (`mapping(bytes4 => struct Timelock.TimelockData)`)
- **performanceFeePercent** (`uint256`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: SizeMetaVault.setPerformanceFeePercent(uint256) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: external
  ├─ [1] ⚙️ FUNCTION: Timelock._updateTimelockStateAndCheckIfTimelocked() (NodeID: 1)
  │   💬 Args: [no args]
  │   👁️  Def: internal
  │ ├─ [2] ⚙️ FUNCTION: Timelock._getTimelockDuration(bytes4) (NodeID: 2)
  │ │   💬 Args: [sig]
  │ │   👁️  Def: internal
  │ │ └─ [3] ⚙️ FUNCTION: Math.max(uint256,uint256) (NodeID: 3)
  │ │     💬 Args: [MINIMUM_TIMELOCK_DURATION, timelockData[sig].duration]
  │ │     👁️  Def: internal
  │ │   └─ [4] ⚙️ FUNCTION: Math.ternary(bool,uint256,uint256) (NodeID: 4)
  │ │       💬 Args: [a > b, a, b]
  │ │       👁️  Def: internal
  │ │     └─ [5] ⚙️ FUNCTION: SafeCast.toUint(bool) (NodeID: 5)
  │ │         💬 Args: [condition]
  │ │         👁️  Def: internal
  │ ├─ [2] ⚙️ FUNCTION: Timelock._getTimelockDuration(bytes4) (NodeID: 6)
  │ │   💬 Args: [sig]
  │ │   👁️  Def: internal
  │ │ └─ [3] ⚙️ FUNCTION: Math.max(uint256,uint256) (NodeID: 7)
  │ │     💬 Args: [MINIMUM_TIMELOCK_DURATION, timelockData[sig].duration]
  │ │     👁️  Def: internal
  │ │   └─ [4] ⚙️ FUNCTION: Math.ternary(bool,uint256,uint256) (NodeID: 8)
  │ │       💬 Args: [a > b, a, b]
  │ │       👁️  Def: internal
  │ │     └─ [5] ⚙️ FUNCTION: SafeCast.toUint(bool) (NodeID: 9)
  │ │         💬 Args: [condition]
  │ │         👁️  Def: internal
  │ └─ [2] ⚙️ FUNCTION: Timelock.isTimelocked(bytes4) (NodeID: 10)
  │     💬 Args: [msg.sig]
  │     👁️  Def: public
  │   └─ [3] ⚙️ FUNCTION: Timelock._getTimelockDuration(bytes4) (NodeID: 11)
  │       💬 Args: [sig]
  │       👁️  Def: internal
  │     └─ [4] ⚙️ FUNCTION: Math.max(uint256,uint256) (NodeID: 12)
  │         💬 Args: [MINIMUM_TIMELOCK_DURATION, timelockData[sig].duration]
  │         👁️  Def: internal
  │       └─ [5] ⚙️ FUNCTION: Math.ternary(bool,uint256,uint256) (NodeID: 13)
  │           💬 Args: [a > b, a, b]
  │           👁️  Def: internal
  │         └─ [6] ⚙️ FUNCTION: SafeCast.toUint(bool) (NodeID: 14)
  │             💬 Args: [condition]
  │             👁️  Def: internal
  ├─ [1] ⚙️ FUNCTION: PerformanceVault._setPerformanceFeePercent(uint256) (NodeID: 15)
  │   💬 Args: [performanceFeePercent_]
  │   👁️  Def: internal
  ├─ [1] 🔒 MODIFIER: BaseVault.onlyAuth(bytes32) (NodeID: 16)
  │   💬 Args: [DEFAULT_ADMIN_ROLE]
  └─ [1] 🔒 MODIFIER: BaseVault.notPaused() (NodeID: 17)
      💬 Args: [no args]
    └─ [2] ⚙️ FUNCTION: PausableUpgradeable.paused() (NodeID: 18)
        💬 Args: [no args]
        👁️  Def: public
      └─ [3] ⚙️ FUNCTION: PausableUpgradeable._getPausableStorage() (NodeID: 19)
          💬 Args: [no args]
          👁️  Def: private
```

## Documentation

### Function Documentation

@notice Sets the performance fee percent
