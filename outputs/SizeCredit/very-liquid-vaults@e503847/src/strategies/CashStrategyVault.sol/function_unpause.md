# Function: unpause()

**Contract**: [src/strategies/CashStrategyVault.sol/contract_CashStrategyVault.md]

## Metadata

- **Contract**: CashStrategyVault
- **Signature**: `unpause()`
- **Visibility**: external
- **Source Range**: 5776:77:56
- **Inherited From**: BaseVault

## Implementation

```solidity
/// @notice Unpauses the vault
///  @dev Only addresses with PAUSER_ROLE can unpause the vault
function unpause() external onlyAuth(PAUSER_ROLE) {
    _unpause();
}
```

## Related Implementations

### _unpause()

- **Kind**: internal
- **Source**: 3478:178:21
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/utils/PausableUpgradeable.sol:PausableUpgradeable:_unpause()`

```solidity
///  @dev Returns to normal state.
///  Requirements:
///  - The contract must be paused.
function _unpause() virtual internal whenPaused() {
    PausableStorage storage $ = _getPausableStorage();
    $._paused = false;
    emit Unpaused(_msgSender());
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

### whenPaused()

- **Kind**: modifier
- **Source**: 2194:66:21
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/utils/PausableUpgradeable.sol:PausableUpgradeable:whenPaused()`

```solidity
///  @dev Modifier to make a function callable only when the contract is paused.
///  Requirements:
///  - The contract must be paused.
modifier whenPaused() {
    _requirePaused();
    _;
}
```

### _requirePaused()

- **Kind**: internal
- **Source**: 2909:126:21
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/utils/PausableUpgradeable.sol:PausableUpgradeable:_requirePaused()`

```solidity
///  @dev Throws if the contract is not paused.
function _requirePaused() virtual internal view {
    if (!paused()) {
        revert ExpectedPause();
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
┌─ [0] ⚙️ FUNCTION: BaseVault.unpause() (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: external
  ├─ [1] ⚙️ FUNCTION: PausableUpgradeable._unpause() (NodeID: 1)
  │   💬 Args: [no args]
  │   👁️  Def: internal
  │ ├─ [2] ⚙️ FUNCTION: PausableUpgradeable._getPausableStorage() (NodeID: 2)
  │ │   💬 Args: [no args]
  │ │   👁️  Def: private
  │ ├─ [2] ⚙️ FUNCTION: ContextUpgradeable._msgSender() (NodeID: 3)
  │ │   💬 Args: [no args]
  │ │   👁️  Def: internal
  │ └─ [2] 🔒 MODIFIER: PausableUpgradeable.whenPaused() (NodeID: 4)
  │     💬 Args: [no args]
  │   └─ [3] ⚙️ FUNCTION: PausableUpgradeable._requirePaused() (NodeID: 5)
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

@notice Unpauses the vault
 @dev Only addresses with PAUSER_ROLE can unpause the vault
