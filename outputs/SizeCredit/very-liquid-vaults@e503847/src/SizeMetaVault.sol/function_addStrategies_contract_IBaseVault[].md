# Function: addStrategies(contract IBaseVault[])

**Contract**: [src/SizeMetaVault.sol/contract_SizeMetaVault.md]

## Metadata

- **Contract**: SizeMetaVault
- **Signature**: `addStrategies(contract IBaseVault[])`
- **Visibility**: external
- **Source Range**: 9441:383:59

## Implementation

```solidity
/// @notice Adds new strategies to the vault
///  @dev Only callable by addresses with STRATEGIST_ROLE
///       If the caller has DEFAULT_ADMIN_ROLE, the timelock state is not updated
function addStrategies(IBaseVault[] calldata strategies_) external notPaused() onlyAuth(STRATEGIST_ROLE) {
    if ((!auth.hasRole(DEFAULT_ADMIN_ROLE, msg.sender)) && _updateTimelockStateAndCheckIfTimelocked()) {
        return;
    }
    for (uint256 i = 0; i < strategies_.length; i++) {
        _addStrategy(strategies_[i], asset(), address(auth));
    }
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

### _addStrategy(contract IBaseVault,address,address)

- **Kind**: internal
- **Source**: 13894:653:59
- **Link**: `src/SizeMetaVault.sol:SizeMetaVault:_addStrategy(contract IBaseVault,address,address)`

```solidity
/// @notice Internal function to add a strategy
///  @dev Strategy configuration is assumed to be correct (non-malicious, no circular dependencies, etc.)
function _addStrategy(IBaseVault strategy_, address asset_, address auth_) private {
    if (address(strategy_) == address(0)) {
        revert NullAddress();
    }
    if (isStrategy(strategy_)) {
        revert InvalidStrategy(address(strategy_));
    }
    if ((strategy_.asset() != asset_) || (address(strategy_.auth()) != auth_)) {
        revert InvalidStrategy(address(strategy_));
    }
    strategies.push(strategy_);
    emit StrategyAdded(address(strategy_));
    if (strategies.length > MAX_STRATEGIES) {
        revert MaxStrategiesExceeded(strategies.length, MAX_STRATEGIES);
    }
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

### isStrategy(contract IBaseVault)

- **Kind**: internal
- **Source**: 19079:286:59
- **Link**: `src/SizeMetaVault.sol:SizeMetaVault:isStrategy(contract IBaseVault)`

```solidity
/// @notice Returns true if the strategy is in the vault
function isStrategy(IBaseVault strategy) public view returns (bool) {
    uint256 length = strategies.length;
    for (uint256 i = 0; i < length; i++) {
        if (strategies[i] == strategy) {
            return true;
        }
    }
    return false;
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

## External Calls

- **Auth::hasRole(bytes32,address)**

## State Variable Reads

- **timelockData** (`mapping(bytes4 => struct Timelock.TimelockData)`)
- **MINIMUM_TIMELOCK_DURATION** (`uint256`)
- **strategies** (`contract IBaseVault[]`) [src/IBaseVault.sol/interface_IBaseVault.md]
- **MAX_STRATEGIES** (`uint256`)
- **auth** (`contract Auth`) [src/utils/Auth.sol/contract_Auth.md]

## State Variable Writes

- **timelockData** (`mapping(bytes4 => struct Timelock.TimelockData)`)
- **strategies** (`contract IBaseVault[]`) [src/IBaseVault.sol/interface_IBaseVault.md]

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: SizeMetaVault.addStrategies(contract IBaseVault[]) (NodeID: 0)
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
  ├─ [1] ⚙️ FUNCTION: SizeMetaVault._addStrategy(contract IBaseVault,address,address) (NodeID: 15)
  │   💬 Args: [strategies_[i], asset(), address(auth)]
  │   👁️  Def: private
  │ ├─ [2] ⚙️ FUNCTION: ERC4626Upgradeable.asset() (NodeID: 17)
  │ │   💬 Args: [no args]
  │ │   👁️  Def: public
  │ │ └─ [3] ⚙️ FUNCTION: ERC4626Upgradeable._getERC4626Storage() (NodeID: 18)
  │ │     💬 Args: [no args]
  │ │     👁️  Def: private
  │ └─ [2] ⚙️ FUNCTION: SizeMetaVault.isStrategy(contract IBaseVault) (NodeID: 16)
  │     💬 Args: [strategy_]
  │     👁️  Def: public
  ├─ [1] 🔒 MODIFIER: BaseVault.onlyAuth(bytes32) (NodeID: 19)
  │   💬 Args: [STRATEGIST_ROLE]
  └─ [1] 🔒 MODIFIER: BaseVault.notPaused() (NodeID: 20)
      💬 Args: [no args]
    └─ [2] ⚙️ FUNCTION: PausableUpgradeable.paused() (NodeID: 21)
        💬 Args: [no args]
        👁️  Def: public
      └─ [3] ⚙️ FUNCTION: PausableUpgradeable._getPausableStorage() (NodeID: 22)
          💬 Args: [no args]
          👁️  Def: private
```

## Documentation

### Function Documentation

@notice Adds new strategies to the vault
 @dev Only callable by addresses with STRATEGIST_ROLE
      If the caller has DEFAULT_ADMIN_ROLE, the timelock state is not updated
