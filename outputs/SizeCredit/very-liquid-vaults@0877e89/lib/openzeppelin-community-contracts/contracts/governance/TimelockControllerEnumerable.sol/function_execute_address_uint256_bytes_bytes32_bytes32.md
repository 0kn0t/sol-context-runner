# Function: execute(address,uint256,bytes,bytes32,bytes32)

**Contract**: [lib/openzeppelin-community-contracts/contracts/governance/TimelockControllerEnumerable.sol/contract_TimelockControllerEnumerable.md]

## Metadata

- **Contract**: TimelockControllerEnumerable
- **Signature**: `execute(address,uint256,bytes,bytes32,bytes32)`
- **Visibility**: public
- **Source Range**: 12174:459:72
- **Inherited From**: TimelockController

## Implementation

```solidity
///  @dev Execute an (ready) operation containing a single transaction.
///  Emits a {CallExecuted} event.
///  Requirements:
///  - the caller must have the 'executor' role.
function execute(address target, uint256 value, bytes calldata payload, bytes32 predecessor, bytes32 salt) virtual public payable onlyRoleOrOpenRole(EXECUTOR_ROLE) {
    bytes32 id = hashOperation(target, value, payload, predecessor, salt);
    _beforeCall(id, predecessor);
    _execute(target, value, payload);
    emit CallExecuted(id, 0, target, value, payload);
    _afterCall(id);
}
```

## Related Implementations

### hashOperation(address,uint256,bytes,bytes32,bytes32)

- **Kind**: internal
- **Source**: 8142:279:72
- **Link**: `lib/openzeppelin-contracts/contracts/governance/TimelockController.sol:TimelockController:hashOperation(address,uint256,bytes,bytes32,bytes32)`

```solidity
///  @dev Returns the identifier of an operation containing a single
///  transaction.
function hashOperation(address target, uint256 value, bytes calldata data, bytes32 predecessor, bytes32 salt) virtual public pure returns (bytes32) {
    return keccak256(abi.encode(target, value, data, predecessor, salt));
}
```

### _beforeCall(bytes32,bytes32)

- **Kind**: internal
- **Source**: 14415:367:72
- **Link**: `lib/openzeppelin-contracts/contracts/governance/TimelockController.sol:TimelockController:_beforeCall(bytes32,bytes32)`

```solidity
///  @dev Checks before execution of an operation's calls.
function _beforeCall(bytes32 id, bytes32 predecessor) private view {
    if (!isOperationReady(id)) {
        revert TimelockUnexpectedOperationState(id, _encodeStateBitmap(OperationState.Ready));
    }
    if ((predecessor != bytes32(0)) && (!isOperationDone(predecessor))) {
        revert TimelockUnexecutedPredecessor(predecessor);
    }
}
```

### isOperationReady(bytes32)

- **Kind**: internal
- **Source**: 6615:134:72
- **Link**: `lib/openzeppelin-contracts/contracts/governance/TimelockController.sol:TimelockController:isOperationReady(bytes32)`

```solidity
///  @dev Returns whether an operation is ready for execution. Note that a "ready" operation is also "pending".
function isOperationReady(bytes32 id) public view returns (bool) {
    return getOperationState(id) == OperationState.Ready;
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

### isOperationDone(bytes32)

- **Kind**: internal
- **Source**: 6828:132:72
- **Link**: `lib/openzeppelin-contracts/contracts/governance/TimelockController.sol:TimelockController:isOperationDone(bytes32)`

```solidity
///  @dev Returns whether an operation is done or not.
function isOperationDone(bytes32 id) public view returns (bool) {
    return getOperationState(id) == OperationState.Done;
}
```

### _execute(address,uint256,bytes)

- **Kind**: internal
- **Source**: 14100:232:72
- **Link**: `lib/openzeppelin-contracts/contracts/governance/TimelockController.sol:TimelockController:_execute(address,uint256,bytes)`

```solidity
///  @dev Execute an operation's call.
function _execute(address target, uint256 value, bytes calldata data) virtual internal {
    (bool success, bytes memory returndata) = target.call{value: value}(data);
    Address.verifyCallResult(success, returndata);
}
```

### verifyCallResult(bool,bytes)

- **Kind**: internal
- **Source**: 5221:224:95
- **Link**: `lib/openzeppelin-contracts/contracts/utils/Address.sol:Address:verifyCallResult(bool,bytes)`

```solidity
///  @dev Tool to verify that a low level call was successful, and reverts if it wasn't, either by bubbling the
///  revert reason or with a default {Errors.FailedCall} error.
function verifyCallResult(bool success, bytes memory returndata) internal pure returns (bytes memory) {
    if (!success) {
        _revert(returndata);
    } else {
        return returndata;
    }
}
```

### _revert(bytes)

- **Kind**: internal
- **Source**: 5559:487:95
- **Link**: `lib/openzeppelin-contracts/contracts/utils/Address.sol:Address:_revert(bytes)`

```solidity
///  @dev Reverts with returndata if present. Otherwise reverts with {Errors.FailedCall}.
function _revert(bytes memory returndata) private pure {
    if (returndata.length > 0) {
        assembly ("memory-safe") {
            let returndata_size := mload(returndata)
            revert(add(32, returndata), returndata_size)
        }
    } else {
        revert Errors.FailedCall();
    }
}
```

### _afterCall(bytes32)

- **Kind**: internal
- **Source**: 14864:236:72
- **Link**: `lib/openzeppelin-contracts/contracts/governance/TimelockController.sol:TimelockController:_afterCall(bytes32)`

```solidity
///  @dev Checks after execution of an operation's calls.
function _afterCall(bytes32 id) private {
    if (!isOperationReady(id)) {
        revert TimelockUnexpectedOperationState(id, _encodeStateBitmap(OperationState.Ready));
    }
    _timestamps[id] = _DONE_TIMESTAMP;
}
```

### onlyRoleOrOpenRole(bytes32)

- **Kind**: modifier
- **Source**: 5291:156:72
- **Link**: `lib/openzeppelin-contracts/contracts/governance/TimelockController.sol:TimelockController:onlyRoleOrOpenRole(bytes32)`

```solidity
///  @dev Modifier to make a function callable only by a certain role. In
///  addition to checking the sender's role, `address(0)` 's role is also
///  considered. Granting a role to `address(0)` is equivalent to enabling
///  this role for everyone.
modifier onlyRoleOrOpenRole(bytes32 role) {
    if (!hasRole(role, address(0))) {
        _checkRole(role, _msgSender());
    }
    _;
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

## State Variable Reads

- **_DONE_TIMESTAMP** (`uint256`)
- **_timestamps** (`mapping(bytes32 => uint256)`)
- **_roles** (`mapping(bytes32 => struct AccessControl.RoleData)`)

## State Variable Writes

- **_timestamps** (`mapping(bytes32 => uint256)`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: TimelockController.execute(address,uint256,bytes,bytes32,bytes32) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
  ├─ [1] ⚙️ FUNCTION: TimelockController.hashOperation(address,uint256,bytes,bytes32,bytes32) (NodeID: 1)
  │   💬 Args: [target, value, payload, predecessor, salt]
  │   👁️  Def: public
  ├─ [1] ⚙️ FUNCTION: TimelockController._beforeCall(bytes32,bytes32) (NodeID: 2)
  │   💬 Args: [id, predecessor]
  │   👁️  Def: private
  │ ├─ [2] ⚙️ FUNCTION: TimelockController.isOperationReady(bytes32) (NodeID: 3)
  │ │   💬 Args: [id]
  │ │   👁️  Def: public
  │ │ └─ [3] ⚙️ FUNCTION: TimelockController.getOperationState(bytes32) (NodeID: 4)
  │ │     💬 Args: [id]
  │ │     👁️  Def: public
  │ │   └─ [4] ⚙️ FUNCTION: TimelockController.getTimestamp(bytes32) (NodeID: 5)
  │ │       💬 Args: [id]
  │ │       👁️  Def: public
  │ ├─ [2] ⚙️ FUNCTION: TimelockController._encodeStateBitmap(enum TimelockController.OperationState) (NodeID: 6)
  │ │   💬 Args: [OperationState.Ready]
  │ │   👁️  Def: internal
  │ └─ [2] ⚙️ FUNCTION: TimelockController.isOperationDone(bytes32) (NodeID: 7)
  │     💬 Args: [predecessor]
  │     👁️  Def: public
  │   └─ [3] ⚙️ FUNCTION: TimelockController.getOperationState(bytes32) (NodeID: 8)
  │       💬 Args: [id]
  │       👁️  Def: public
  │     └─ [4] ⚙️ FUNCTION: TimelockController.getTimestamp(bytes32) (NodeID: 9)
  │         💬 Args: [id]
  │         👁️  Def: public
  ├─ [1] ⚙️ FUNCTION: TimelockController._execute(address,uint256,bytes) (NodeID: 10)
  │   💬 Args: [target, value, payload]
  │   👁️  Def: internal
  │ └─ [2] ⚙️ FUNCTION: Address.verifyCallResult(bool,bytes) (NodeID: 11)
  │     💬 Args: [success, returndata]
  │     👁️  Def: internal
  │   └─ [3] ⚙️ FUNCTION: Address._revert(bytes) (NodeID: 12)
  │       💬 Args: [returndata]
  │       👁️  Def: private
  ├─ [1] ⚙️ FUNCTION: TimelockController._afterCall(bytes32) (NodeID: 13)
  │   💬 Args: [id]
  │   👁️  Def: private
  │ ├─ [2] ⚙️ FUNCTION: TimelockController.isOperationReady(bytes32) (NodeID: 14)
  │ │   💬 Args: [id]
  │ │   👁️  Def: public
  │ │ └─ [3] ⚙️ FUNCTION: TimelockController.getOperationState(bytes32) (NodeID: 15)
  │ │     💬 Args: [id]
  │ │     👁️  Def: public
  │ │   └─ [4] ⚙️ FUNCTION: TimelockController.getTimestamp(bytes32) (NodeID: 16)
  │ │       💬 Args: [id]
  │ │       👁️  Def: public
  │ └─ [2] ⚙️ FUNCTION: TimelockController._encodeStateBitmap(enum TimelockController.OperationState) (NodeID: 17)
  │     💬 Args: [OperationState.Ready]
  │     👁️  Def: internal
  └─ [1] 🔒 MODIFIER: TimelockController.onlyRoleOrOpenRole(bytes32) (NodeID: 18)
      💬 Args: [EXECUTOR_ROLE]
    ├─ [2] ⚙️ FUNCTION: AccessControl.hasRole(bytes32,address) (NodeID: 19)
    │   💬 Args: [EXECUTOR_ROLE, address(0)]
    │   👁️  Def: public
    └─ [2] ⚙️ FUNCTION: AccessControl._checkRole(bytes32,address) (NodeID: 20)
        💬 Args: [EXECUTOR_ROLE, _msgSender()]
        👁️  Def: internal
      ├─ [3] ⚙️ FUNCTION: Context._msgSender() (NodeID: 22)
      │   💬 Args: [no args]
      │   👁️  Def: internal
      └─ [3] ⚙️ FUNCTION: AccessControl.hasRole(bytes32,address) (NodeID: 21)
          💬 Args: [role, account]
          👁️  Def: public
```

## Documentation

### Function Documentation

 @dev Execute an (ready) operation containing a single transaction.
 Emits a {CallExecuted} event.
 Requirements:
 - the caller must have the 'executor' role.
