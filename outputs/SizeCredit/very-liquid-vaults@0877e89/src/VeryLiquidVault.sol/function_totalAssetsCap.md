# Function: totalAssetsCap()

**Contract**: [src/VeryLiquidVault.sol/contract_VeryLiquidVault.md]

## Metadata

- **Contract**: VeryLiquidVault
- **Signature**: `totalAssetsCap()`
- **Visibility**: public
- **Source Range**: 14617:123:149
- **Inherited From**: BaseVault

## Implementation

```solidity
/// @inheritdoc IVault
function totalAssetsCap() override public view nonReentrantView() returns (uint256) {
    return _totalAssetsCap();
}
```

## Related Implementations

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
┌─ [0] ⚙️ FUNCTION: BaseVault.totalAssetsCap() (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
  ├─ [1] ⚙️ FUNCTION: BaseVault._totalAssetsCap() (NodeID: 1)
  │   💬 Args: [no args]
  │   👁️  Def: private
  │ └─ [2] ⚙️ FUNCTION: BaseVault._getBaseVaultStorage() (NodeID: 2)
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

@inheritdoc IVault

### Interface Documentation

@notice Returns the maximum amount of underlying assets that the vault can hold.
 @dev Expressed in units of the underlying ERC20 `asset()`. A value of type(uint256).max means no cap is enforced. This limit is used to restrict deposits/mints to avoid excessive exposure.
 @dev The vault's totalAssets can get higher than the cap in case of donations, accrued yield, etc.
 @return The maximum totalAssets allowed in the vault
