# Function: setFeeRecipient(address)

**Contract**: [src/SizeMetaVault.sol/contract_SizeMetaVault.md]

## Metadata

- **Contract**: SizeMetaVault
- **Signature**: `setFeeRecipient(address)`
- **Visibility**: external
- **Source Range**: 7610:144:59

## Implementation

```solidity
/// @notice Sets the fee recipient
function setFeeRecipient(address feeRecipient_) external notPaused() onlyAuth(DEFAULT_ADMIN_ROLE) {
    _setFeeRecipient(feeRecipient_);
}
```

## Related Implementations

### _setFeeRecipient(address)

- **Kind**: internal
- **Source**: 3655:219:58
- **Link**: `src/PerformanceVault.sol:PerformanceVault:_setFeeRecipient(address)`

```solidity
/// @notice Sets the fee recipient
function _setFeeRecipient(address feeRecipient_) internal {
    address feeRecipientBefore = feeRecipient;
    feeRecipient = feeRecipient_;
    emit FeeRecipientSet(feeRecipientBefore, feeRecipient_);
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

- **feeRecipient** (`address`)
- **auth** (`contract Auth`) [src/utils/Auth.sol/contract_Auth.md]

## State Variable Writes

- **feeRecipient** (`address`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: SizeMetaVault.setFeeRecipient(address) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: external
  ├─ [1] ⚙️ FUNCTION: PerformanceVault._setFeeRecipient(address) (NodeID: 1)
  │   💬 Args: [feeRecipient_]
  │   👁️  Def: internal
  ├─ [1] 🔒 MODIFIER: BaseVault.onlyAuth(bytes32) (NodeID: 2)
  │   💬 Args: [DEFAULT_ADMIN_ROLE]
  └─ [1] 🔒 MODIFIER: BaseVault.notPaused() (NodeID: 3)
      💬 Args: [no args]
    └─ [2] ⚙️ FUNCTION: PausableUpgradeable.paused() (NodeID: 4)
        💬 Args: [no args]
        👁️  Def: public
      └─ [3] ⚙️ FUNCTION: PausableUpgradeable._getPausableStorage() (NodeID: 5)
          💬 Args: [no args]
          👁️  Def: private
```

## Documentation

### Function Documentation

@notice Sets the fee recipient
