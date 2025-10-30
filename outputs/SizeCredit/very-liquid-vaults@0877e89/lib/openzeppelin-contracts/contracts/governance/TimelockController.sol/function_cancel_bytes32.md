# Function: cancel(bytes32)

**Contract**: [lib/openzeppelin-contracts/contracts/governance/TimelockController.sol/contract_TimelockController.md]

## Metadata

- **Contract**: TimelockController
- **Signature**: `cancel(bytes32)`
- **Visibility**: public
- **Source Range**: 11325:375:72

## Implementation

```solidity
///  @dev Cancel an operation.
///  Requirements:
///  - the caller must have the 'canceller' role.
function cancel(bytes32 id) virtual public onlyRole(CANCELLER_ROLE) {
    if (!isOperationPending(id)) {
        revert TimelockUnexpectedOperationState(id, _encodeStateBitmap(OperationState.Waiting) | _encodeStateBitmap(OperationState.Ready));
    }
    delete _timestamps[id];
    emit Cancelled(id);
}
```

## Related Implementations

### isOperationPending(bytes32)

- **Kind**: internal
- **Source**: 6270:209:72
- **Link**: `lib/openzeppelin-contracts/contracts/governance/TimelockController.sol:TimelockController:isOperationPending(bytes32)`

```solidity
///  @dev Returns whether an operation is pending or not. Note that a "pending" operation may also be "ready".
function isOperationPending(bytes32 id) public view returns (bool) {
    OperationState state = getOperationState(id);
    return (state == OperationState.Waiting) || (state == OperationState.Ready);
}
```

### getOperationState(bytes32)

- **Kind**: internal
- **Source**: 7278:460:72
- **Link**: `lib/openzeppelin-contracts/contracts/governance/TimelockController.sol:TimelockController:getOperationState(bytes32)`

```solidity
///  @dev Returns operation state.
function getOperationState(bytes32 id) virtual public view returns (OperationState) {
    uint256 timestamp = getTimestamp(id);
    if (timestamp == 0) {
        return OperationState.Unset;
    } else if (timestamp == _DONE_TIMESTAMP) {
        return OperationState.Done;
    } else if (timestamp > block.timestamp) {
        return OperationState.Waiting;
    } else {
        return OperationState.Ready;
    }
}
```

### getTimestamp(bytes32)

- **Kind**: internal
- **Source**: 7108:111:72
- **Link**: `lib/openzeppelin-contracts/contracts/governance/TimelockController.sol:TimelockController:getTimestamp(bytes32)`

```solidity
///  @dev Returns the timestamp at which an operation becomes ready (0 for
///  unset operations, 1 for done operations).
function getTimestamp(bytes32 id) virtual public view returns (uint256) {
    return _timestamps[id];
}
```

### _encodeStateBitmap(enum TimelockController.OperationState)

- **Kind**: internal
- **Source**: 16145:150:72
- **Link**: `lib/openzeppelin-contracts/contracts/governance/TimelockController.sol:TimelockController:_encodeStateBitmap(enum TimelockController.OperationState)`

```solidity
///  @dev Encodes a `OperationState` into a `bytes32` representation where each bit enabled corresponds to
///  the underlying position in the `OperationState` enum. For example:
///  0x000...1000
///    ^^^^^^----- ...
///          ^---- Done
///           ^--- Ready
///            ^-- Waiting
///             ^- Unset
function _encodeStateBitmap(OperationState operationState) internal pure returns (bytes32) {
    return bytes32(1 << uint8(operationState));
}
```

### onlyRole(bytes32)

- **Kind**: modifier
- **Source**: 2422:76:68
- **Link**: `lib/openzeppelin-contracts/contracts/access/AccessControl.sol:AccessControl:onlyRole(bytes32)`

```solidity
///  @dev Modifier that checks that an account has a specific role. Reverts
///  with an {AccessControlUnauthorizedAccount} error including the required role.
modifier onlyRole(bytes32 role) {
    _checkRole(role);
    _;
}
```

### _checkRole(bytes32)

- **Kind**: internal
- **Source**: 3199:103:68
- **Link**: `lib/openzeppelin-contracts/contracts/access/AccessControl.sol:AccessControl:_checkRole(bytes32)`

```solidity
///  @dev Reverts with an {AccessControlUnauthorizedAccount} error if `_msgSender()`
///  is missing `role`. Overriding this function changes the behavior of the {onlyRole} modifier.
function _checkRole(bytes32 role) virtual internal view {
    _checkRole(role, _msgSender());
}
```

### _checkRole(bytes32,address)

- **Kind**: internal
- **Source**: 3432:197:68
- **Link**: `lib/openzeppelin-contracts/contracts/access/AccessControl.sol:AccessControl:_checkRole(bytes32,address)`

```solidity
///  @dev Reverts with an {AccessControlUnauthorizedAccount} error if `account`
///  is missing `role`.
function _checkRole(bytes32 role, address account) virtual internal view {
    if (!hasRole(role, account)) {
        revert AccessControlUnauthorizedAccount(account, role);
    }
}
```

### _msgSender()

- **Kind**: internal
- **Source**: 656:96:98
- **Link**: `lib/openzeppelin-contracts/contracts/utils/Context.sol:Context:_msgSender()`

```solidity
function _msgSender() virtual internal view returns (address) {
    return msg.sender;
}
```

### hasRole(bytes32,address)

- **Kind**: internal
- **Source**: 2854:136:68
- **Link**: `lib/openzeppelin-contracts/contracts/access/AccessControl.sol:AccessControl:hasRole(bytes32,address)`

```solidity
///  @dev Returns `true` if `account` has been granted `role`.
function hasRole(bytes32 role, address account) virtual public view returns (bool) {
    return _roles[role].hasRole[account];
}
```

## State Variable Reads

- **_DONE_TIMESTAMP** (`uint256`)
- **_timestamps** (`mapping(bytes32 => uint256)`)
- **_roles** (`mapping(bytes32 => struct AccessControl.RoleData)`)

## State Variable Writes

- **_timestamps** (`mapping(bytes32 => uint256)`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: TimelockController.cancel(bytes32) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
  ├─ [1] ⚙️ FUNCTION: TimelockController.isOperationPending(bytes32) (NodeID: 1)
  │   💬 Args: [id]
  │   👁️  Def: public
  │ └─ [2] ⚙️ FUNCTION: TimelockController.getOperationState(bytes32) (NodeID: 2)
  │     💬 Args: [id]
  │     👁️  Def: public
  │   └─ [3] ⚙️ FUNCTION: TimelockController.getTimestamp(bytes32) (NodeID: 3)
  │       💬 Args: [id]
  │       👁️  Def: public
  ├─ [1] ⚙️ FUNCTION: TimelockController._encodeStateBitmap(enum TimelockController.OperationState) (NodeID: 4)
  │   💬 Args: [OperationState.Ready]
  │   👁️  Def: internal
  ├─ [1] ⚙️ FUNCTION: TimelockController._encodeStateBitmap(enum TimelockController.OperationState) (NodeID: 5)
  │   💬 Args: [OperationState.Waiting]
  │   👁️  Def: internal
  └─ [1] 🔒 MODIFIER: AccessControl.onlyRole(bytes32) (NodeID: 6)
      💬 Args: [CANCELLER_ROLE]
    └─ [2] ⚙️ FUNCTION: AccessControl._checkRole(bytes32) (NodeID: 7)
        💬 Args: [CANCELLER_ROLE]
        👁️  Def: internal
      └─ [3] ⚙️ FUNCTION: AccessControl._checkRole(bytes32,address) (NodeID: 8)
          💬 Args: [role, _msgSender()]
          👁️  Def: internal
        ├─ [4] ⚙️ FUNCTION: Context._msgSender() (NodeID: 10)
        │   💬 Args: [no args]
        │   👁️  Def: internal
        └─ [4] ⚙️ FUNCTION: AccessControl.hasRole(bytes32,address) (NodeID: 9)
            💬 Args: [role, account]
            👁️  Def: public
```

## Documentation

### Function Documentation

 @dev Cancel an operation.
 Requirements:
 - the caller must have the 'canceller' role.
