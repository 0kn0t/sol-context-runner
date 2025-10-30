# Function: reorderStrategies(contract IBaseVault[])

**Contract**: [src/SizeMetaVault.sol/contract_SizeMetaVault.md]

## Metadata

- **Contract**: SizeMetaVault
- **Signature**: `reorderStrategies(contract IBaseVault[])`
- **Visibility**: external
- **Source Range**: 8153:1087:59

## Implementation

```solidity
/// @notice Reorders the strategies
///  @dev Verifies that the new strategies order is valid and that there are no duplicates
///  @dev Clears current strategies and adds them in the new order
function reorderStrategies(IBaseVault[] calldata newStrategiesOrder) external notPaused() onlyAuth(STRATEGIST_ROLE) {
    if (strategies.length != newStrategiesOrder.length) {
        revert ArrayLengthMismatch(strategies.length, newStrategiesOrder.length);
    }
    for (uint256 i = 0; i < newStrategiesOrder.length; i++) {
        if (!isStrategy(newStrategiesOrder[i])) {
            revert InvalidStrategy(address(newStrategiesOrder[i]));
        }
        for (uint256 j = i + 1; j < newStrategiesOrder.length; j++) {
            if (newStrategiesOrder[i] == newStrategiesOrder[j]) {
                revert InvalidStrategy(address(newStrategiesOrder[i]));
            }
        }
    }
    IBaseVault[] memory oldStrategiesOrder = strategies;
    for (uint256 i = 0; i < oldStrategiesOrder.length; i++) {
        _removeStrategy(oldStrategiesOrder[i]);
    }
    for (uint256 i = 0; i < newStrategiesOrder.length; i++) {
        _addStrategy(newStrategiesOrder[i], asset(), address(auth));
    }
}
```

## Related Implementations

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

### _removeStrategy(contract IBaseVault)

- **Kind**: internal
- **Source**: 14799:596:59
- **Link**: `src/SizeMetaVault.sol:SizeMetaVault:_removeStrategy(contract IBaseVault)`

```solidity
/// @notice Internal function to remove a strategy
///  @dev No NullAddress check is needed because only whitelisted strategies can be removed, and it is checked in _addStrategy
///  @dev Removes the strategy in-place to keep the order
function _removeStrategy(IBaseVault strategy) private {
    bool removed = false;
    for (uint256 i = 0; i < strategies.length; i++) {
        if (strategies[i] == strategy) {
            for (uint256 j = i; j < (strategies.length - 1); j++) {
                strategies[j] = strategies[j + 1];
            }
            strategies.pop();
            emit StrategyRemoved(address(strategy));
            removed = true;
            break;
        }
    }
    if (!removed) {
        revert InvalidStrategy(address(strategy));
    }
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

- **strategies** (`contract IBaseVault[]`) [src/IBaseVault.sol/interface_IBaseVault.md]
- **MAX_STRATEGIES** (`uint256`)
- **auth** (`contract Auth`) [src/utils/Auth.sol/contract_Auth.md]

## State Variable Writes

- **strategies** (`contract IBaseVault[]`) [src/IBaseVault.sol/interface_IBaseVault.md]

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: SizeMetaVault.reorderStrategies(contract IBaseVault[]) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: external
  ├─ [1] ⚙️ FUNCTION: SizeMetaVault.isStrategy(contract IBaseVault) (NodeID: 1)
  │   💬 Args: [newStrategiesOrder[i]]
  │   👁️  Def: public
  ├─ [1] ⚙️ FUNCTION: SizeMetaVault._removeStrategy(contract IBaseVault) (NodeID: 2)
  │   💬 Args: [oldStrategiesOrder[i]]
  │   👁️  Def: private
  ├─ [1] ⚙️ FUNCTION: SizeMetaVault._addStrategy(contract IBaseVault,address,address) (NodeID: 3)
  │   💬 Args: [newStrategiesOrder[i], asset(), address(auth)]
  │   👁️  Def: private
  │ ├─ [2] ⚙️ FUNCTION: ERC4626Upgradeable.asset() (NodeID: 5)
  │ │   💬 Args: [no args]
  │ │   👁️  Def: public
  │ │ └─ [3] ⚙️ FUNCTION: ERC4626Upgradeable._getERC4626Storage() (NodeID: 6)
  │ │     💬 Args: [no args]
  │ │     👁️  Def: private
  │ └─ [2] ⚙️ FUNCTION: SizeMetaVault.isStrategy(contract IBaseVault) (NodeID: 4)
  │     💬 Args: [strategy_]
  │     👁️  Def: public
  ├─ [1] 🔒 MODIFIER: BaseVault.onlyAuth(bytes32) (NodeID: 7)
  │   💬 Args: [STRATEGIST_ROLE]
  └─ [1] 🔒 MODIFIER: BaseVault.notPaused() (NodeID: 8)
      💬 Args: [no args]
    └─ [2] ⚙️ FUNCTION: PausableUpgradeable.paused() (NodeID: 9)
        💬 Args: [no args]
        👁️  Def: public
      └─ [3] ⚙️ FUNCTION: PausableUpgradeable._getPausableStorage() (NodeID: 10)
          💬 Args: [no args]
          👁️  Def: private
```

## Documentation

### Function Documentation

@notice Reorders the strategies
 @dev Verifies that the new strategies order is valid and that there are no duplicates
 @dev Clears current strategies and adds them in the new order
