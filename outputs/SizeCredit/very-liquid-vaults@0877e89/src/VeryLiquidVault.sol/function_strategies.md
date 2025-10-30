# Function: strategies()

**Contract**: [src/VeryLiquidVault.sol/contract_VeryLiquidVault.md]

## Metadata

- **Contract**: VeryLiquidVault
- **Signature**: `strategies()`
- **Visibility**: public
- **Source Range**: 24032:114:145

## Implementation

```solidity
/// @notice Returns the strategies in the vault
///  @return The strategies in the vault
function strategies() public view nonReentrantView() returns (IVault[] memory) {
    return _strategies();
}
```

## Related Implementations

### _strategies()

- **Kind**: internal
- **Source**: 24221:126:145
- **Link**: `src/VeryLiquidVault.sol:VeryLiquidVault:_strategies()`

```solidity
/// @notice Internal function to get the strategies in the vault
function _strategies() private view returns (IVault[] memory) {
    return _getVeryLiquidVaultStorage()._strategies;
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
┌─ [0] ⚙️ FUNCTION: VeryLiquidVault.strategies() (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
  ├─ [1] ⚙️ FUNCTION: VeryLiquidVault._strategies() (NodeID: 1)
  │   💬 Args: [no args]
  │   👁️  Def: private
  │ └─ [2] ⚙️ FUNCTION: VeryLiquidVault._getVeryLiquidVaultStorage() (NodeID: 2)
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

@notice Returns the strategies in the vault
 @return The strategies in the vault
