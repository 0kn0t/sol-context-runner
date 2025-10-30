# Function: highWaterMark()

**Contract**: [src/VeryLiquidVault.sol/contract_VeryLiquidVault.md]

## Metadata

- **Contract**: VeryLiquidVault
- **Signature**: `highWaterMark()`
- **Visibility**: public
- **Source Range**: 7789:112:151
- **Inherited From**: PerformanceVault

## Implementation

```solidity
/// @notice Returns the high water mark
function highWaterMark() public view nonReentrantView() returns (uint256) {
    return _highWaterMark();
}
```

## Related Implementations

### _highWaterMark()

- **Kind**: internal
- **Source**: 7971:125:151
- **Link**: `src/utils/PerformanceVault.sol:PerformanceVault:_highWaterMark()`

```solidity
/// @notice Internal function to return the high water mark
function _highWaterMark() private view returns (uint256) {
    return _getPerformanceVaultStorage()._highWaterMark;
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

### nonReentrantView()

- **Kind**: modifier
- **Source**: 354:81:152
- **Link**: `src/utils/ReentrancyGuardUpgradeableWithViewModifier.sol:ReentrancyGuardUpgradeableWithViewModifier:nonReentrantView()`

```solidity
/// @dev See https://github.com/OpenZeppelin/openzeppelin-contracts/pull/5800
modifier nonReentrantView() {
    _nonReentrantBeforeView();
    _;
}
```

### _nonReentrantBeforeView()

- **Kind**: internal
- **Source**: 523:133:152
- **Link**: `src/utils/ReentrancyGuardUpgradeableWithViewModifier.sol:ReentrancyGuardUpgradeableWithViewModifier:_nonReentrantBeforeView()`

```solidity
/// @dev See https://github.com/OpenZeppelin/openzeppelin-contracts/pull/5800
function _nonReentrantBeforeView() private view {
    if (_reentrancyGuardEntered()) revert ReentrancyGuardReentrantCall();
}
```

### _reentrancyGuardEntered()

- **Kind**: internal
- **Source**: 4322:181:65
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/utils/ReentrancyGuardUpgradeable.sol:ReentrancyGuardUpgradeable:_reentrancyGuardEntered()`

```solidity
///  @dev Returns true if the reentrancy guard is currently set to "entered", which indicates there is a
///  `nonReentrant` function in the call stack.
function _reentrancyGuardEntered() internal view returns (bool) {
    ReentrancyGuardStorage storage $ = _getReentrancyGuardStorage();
    return $._status == ENTERED;
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

## State Variable Reads

- **ENTERED** (`uint256`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: PerformanceVault.highWaterMark() (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
  ├─ [1] ⚙️ FUNCTION: PerformanceVault._highWaterMark() (NodeID: 1)
  │   💬 Args: [no args]
  │   👁️  Def: private
  │ └─ [2] ⚙️ FUNCTION: PerformanceVault._getPerformanceVaultStorage() (NodeID: 2)
  │     💬 Args: [no args]
  │     👁️  Def: private
  └─ [1] 🔒 MODIFIER: ReentrancyGuardUpgradeableWithViewModifier.nonReentrantView() (NodeID: 3)
      💬 Args: [no args]
    └─ [2] ⚙️ FUNCTION: ReentrancyGuardUpgradeableWithViewModifier._nonReentrantBeforeView() (NodeID: 4)
        💬 Args: [no args]
        👁️  Def: private
      └─ [3] ⚙️ FUNCTION: ReentrancyGuardUpgradeable._reentrancyGuardEntered() (NodeID: 5)
          💬 Args: [no args]
          👁️  Def: internal
        └─ [4] ⚙️ FUNCTION: ReentrancyGuardUpgradeable._getReentrancyGuardStorage() (NodeID: 6)
            💬 Args: [no args]
            👁️  Def: private
```

## Documentation

### Function Documentation

@notice Returns the high water mark
