# Function: pause()

**Contract**: [src/SizeMetaVault.sol/contract_SizeMetaVault.md]

## Metadata

- **Contract**: SizeMetaVault
- **Signature**: `pause()`
- **Visibility**: external
- **Source Range**: 5595:73:56
- **Inherited From**: BaseVault

## Implementation

```solidity
/// @notice Pauses the vault
///  @dev Only addresses with PAUSER_ROLE can pause the vault
function pause() external onlyAuth(PAUSER_ROLE) {
    _pause();
}
```

## Related Implementations

### _pause()

- **Kind**: internal
- **Source**: 3170:176:21
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/utils/PausableUpgradeable.sol:PausableUpgradeable:_pause()`

```solidity
///  @dev Triggers stopped state.
///  Requirements:
///  - The contract must not be paused.
function _pause() virtual internal whenNotPaused() {
    PausableStorage storage $ = _getPausableStorage();
    $._paused = true;
    emit Paused(_msgSender());
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

### _msgSender()

- **Kind**: internal
- **Source**: 887:96:18
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/utils/ContextUpgradeable.sol:ContextUpgradeable:_msgSender()`

```solidity
function _msgSender() virtual internal view returns (address) {
    return msg.sender;
}
```

### whenNotPaused()

- **Kind**: modifier
- **Source**: 1944:72:21
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/utils/PausableUpgradeable.sol:PausableUpgradeable:whenNotPaused()`

```solidity
///  @dev Modifier to make a function callable only when the contract is not paused.
///  Requirements:
///  - The contract must not be paused.
modifier whenNotPaused() {
    _requireNotPaused();
    _;
}
```

### _requireNotPaused()

- **Kind**: internal
- **Source**: 2709:128:21
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/utils/PausableUpgradeable.sol:PausableUpgradeable:_requireNotPaused()`

```solidity
///  @dev Throws if the contract is paused.
function _requireNotPaused() virtual internal view {
    if (paused()) {
        revert EnforcedPause();
    }
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

## State Variable Reads

- **auth** (`contract Auth`) [src/utils/Auth.sol/contract_Auth.md]

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: BaseVault.pause() (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: external
  ├─ [1] ⚙️ FUNCTION: PausableUpgradeable._pause() (NodeID: 1)
  │   💬 Args: [no args]
  │   👁️  Def: internal
  │ ├─ [2] ⚙️ FUNCTION: PausableUpgradeable._getPausableStorage() (NodeID: 2)
  │ │   💬 Args: [no args]
  │ │   👁️  Def: private
  │ ├─ [2] ⚙️ FUNCTION: ContextUpgradeable._msgSender() (NodeID: 3)
  │ │   💬 Args: [no args]
  │ │   👁️  Def: internal
  │ └─ [2] 🔒 MODIFIER: PausableUpgradeable.whenNotPaused() (NodeID: 4)
  │     💬 Args: [no args]
  │   └─ [3] ⚙️ FUNCTION: PausableUpgradeable._requireNotPaused() (NodeID: 5)
  │       💬 Args: [no args]
  │       👁️  Def: internal
  │     └─ [4] ⚙️ FUNCTION: PausableUpgradeable.paused() (NodeID: 6)
  │         💬 Args: [no args]
  │         👁️  Def: public
  │       └─ [5] ⚙️ FUNCTION: PausableUpgradeable._getPausableStorage() (NodeID: 7)
  │           💬 Args: [no args]
  │           👁️  Def: private
  └─ [1] 🔒 MODIFIER: BaseVault.onlyAuth(bytes32) (NodeID: 8)
      💬 Args: [PAUSER_ROLE]
```

## Documentation

### Function Documentation

@notice Pauses the vault
 @dev Only addresses with PAUSER_ROLE can pause the vault
