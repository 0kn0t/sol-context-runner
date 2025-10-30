# Function: setTimelockDuration(bytes4,uint256)

**Contract**: [src/SizeMetaVault.sol/contract_SizeMetaVault.md]

## Metadata

- **Contract**: SizeMetaVault
- **Signature**: `setTimelockDuration(bytes4,uint256)`
- **Visibility**: external
- **Source Range**: 7083:159:59

## Implementation

```solidity
/// @notice Sets the timelock duration for a specific function
///  @dev Only callable by addresses with DEFAULT_ADMIN_ROLE
///       The admin cannot update the timelock duration for setPerformanceFeePercent, except through a contract upgrade
function setTimelockDuration(bytes4 sig, uint256 duration) external notPaused() onlyAuth(DEFAULT_ADMIN_ROLE) {
    _setTimelockDuration(sig, duration);
}
```

## Related Implementations

### _setTimelockDuration(bytes4,uint256)

- **Kind**: internal
- **Source**: 4200:151:64
- **Link**: `src/utils/Timelock.sol:Timelock:_setTimelockDuration(bytes4,uint256)`

```solidity
/// @notice Sets the timelock duration for a specific function
///  @dev Updating the timelock duration has immediate effect on the timelocked functions
function _setTimelockDuration(bytes4 sig, uint256 duration) internal {
    _setTimelockDuration(sig, duration, timelockData[sig].isEditable);
}
```

### _setTimelockDuration(bytes4,uint256,bool)

- **Kind**: internal
- **Source**: 3421:613:64
- **Link**: `src/utils/Timelock.sol:Timelock:_setTimelockDuration(bytes4,uint256,bool)`

```solidity
/// @notice Sets the timelock duration for a specific function
///  @dev Updating the timelock duration has immediate effect on the timelocked functions
function _setTimelockDuration(bytes4 sig, uint256 duration, bool isEditable) internal {
    if (duration < MINIMUM_TIMELOCK_DURATION) {
        revert TimelockDurationTooShort(sig, duration, MINIMUM_TIMELOCK_DURATION);
    }
    if ((timelockData[sig].duration > 0) && (!timelockData[sig].isEditable)) {
        revert TimelockNotEditable(sig);
    }
    uint256 durationBefore = timelockData[sig].duration;
    timelockData[sig].duration = duration;
    timelockData[sig].isEditable = isEditable;
    emit TimelockDurationSet(sig, isEditable, durationBefore, duration);
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
- **auth** (`contract Auth`) [src/utils/Auth.sol/contract_Auth.md]

## State Variable Writes

- **timelockData** (`mapping(bytes4 => struct Timelock.TimelockData)`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: SizeMetaVault.setTimelockDuration(bytes4,uint256) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: external
  ├─ [1] ⚙️ FUNCTION: Timelock._setTimelockDuration(bytes4,uint256) (NodeID: 1)
  │   💬 Args: [sig, duration]
  │   👁️  Def: internal
  │ └─ [2] ⚙️ FUNCTION: Timelock._setTimelockDuration(bytes4,uint256,bool) (NodeID: 2)
  │     💬 Args: [sig, duration, timelockData[sig].isEditable]
  │     👁️  Def: internal
  ├─ [1] 🔒 MODIFIER: BaseVault.onlyAuth(bytes32) (NodeID: 3)
  │   💬 Args: [DEFAULT_ADMIN_ROLE]
  └─ [1] 🔒 MODIFIER: BaseVault.notPaused() (NodeID: 4)
      💬 Args: [no args]
    └─ [2] ⚙️ FUNCTION: PausableUpgradeable.paused() (NodeID: 5)
        💬 Args: [no args]
        👁️  Def: public
      └─ [3] ⚙️ FUNCTION: PausableUpgradeable._getPausableStorage() (NodeID: 6)
          💬 Args: [no args]
          👁️  Def: private
```

## Documentation

### Function Documentation

@notice Sets the timelock duration for a specific function
 @dev Only callable by addresses with DEFAULT_ADMIN_ROLE
      The admin cannot update the timelock duration for setPerformanceFeePercent, except through a contract upgrade
