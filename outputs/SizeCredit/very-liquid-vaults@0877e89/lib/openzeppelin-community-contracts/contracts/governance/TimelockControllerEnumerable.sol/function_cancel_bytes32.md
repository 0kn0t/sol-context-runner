# Function: cancel(bytes32)

**Contract**: [lib/openzeppelin-community-contracts/contracts/governance/TimelockControllerEnumerable.sol/contract_TimelockControllerEnumerable.md]

## Metadata

- **Contract**: TimelockControllerEnumerable
- **Signature**: `cancel(bytes32)`
- **Visibility**: public
- **Source Range**: 3495:370:53

## Implementation

```solidity
/// @inheritdoc TimelockController
///  @dev Remove the operation
function cancel(bytes32 id) virtual override public {
    super.cancel(id);
    if (_operationsIdSet.contains(id)) {
        _operationsIdSet.remove(id);
        delete _operationsMap[id];
    }
    if (_operationsBatchIdSet.contains(id)) {
        _operationsBatchIdSet.remove(id);
        delete _operationsBatchMap[id];
    }
}
```

## Related Implementations

### cancel(bytes32)

- **Kind**: internal
- **Source**: 11325:375:72
- **Link**: `lib/openzeppelin-contracts/contracts/governance/TimelockController.sol:TimelockController:cancel(bytes32)`

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

### contains(struct EnumerableSet.Bytes32Set,bytes32)

- **Kind**: internal
- **Source**: 7474:138:112
- **Link**: `lib/openzeppelin-contracts/contracts/utils/structs/EnumerableSet.sol:EnumerableSet:contains(struct EnumerableSet.Bytes32Set,bytes32)`

```solidity
///  @dev Returns true if the value is in the set. O(1).
function contains(Bytes32Set storage set, bytes32 value) internal view returns (bool) {
    return _contains(set._inner, value);
}
```

### _contains(struct EnumerableSet.Set,bytes32)

- **Kind**: internal
- **Source**: 4910:129:112
- **Link**: `lib/openzeppelin-contracts/contracts/utils/structs/EnumerableSet.sol:EnumerableSet:_contains(struct EnumerableSet.Set,bytes32)`

```solidity
///  @dev Returns true if the value is in the set. O(1).
function _contains(Set storage set, bytes32 value) private view returns (bool) {
    return set._positions[value] != 0;
}
```

### remove(struct EnumerableSet.Bytes32Set,bytes32)

- **Kind**: internal
- **Source**: 6867:129:112
- **Link**: `lib/openzeppelin-contracts/contracts/utils/structs/EnumerableSet.sol:EnumerableSet:remove(struct EnumerableSet.Bytes32Set,bytes32)`

```solidity
///  @dev Removes a value from a set. O(1).
///  Returns true if the value was removed from the set, that is if it was
///  present.
function remove(Bytes32Set storage set, bytes32 value) internal returns (bool) {
    return _remove(set._inner, value);
}
```

### _remove(struct EnumerableSet.Set,bytes32)

- **Kind**: internal
- **Source**: 2910:1368:112
- **Link**: `lib/openzeppelin-contracts/contracts/utils/structs/EnumerableSet.sol:EnumerableSet:_remove(struct EnumerableSet.Set,bytes32)`

```solidity
///  @dev Removes a value from a set. O(1).
///  Returns true if the value was removed from the set, that is if it was
///  present.
function _remove(Set storage set, bytes32 value) private returns (bool) {
    uint256 position = set._positions[value];
    if (position != 0) {
        uint256 valueIndex = position - 1;
        uint256 lastIndex = set._values.length - 1;
        if (valueIndex != lastIndex) {
            bytes32 lastValue = set._values[lastIndex];
            set._values[valueIndex] = lastValue;
            set._positions[lastValue] = position;
        }
        set._values.pop();
        delete set._positions[value];
        return true;
    } else {
        return false;
    }
}
```

## State Variable Reads

- **_operationsIdSet** (`struct EnumerableSet.Bytes32Set`)
- **_operationsBatchIdSet** (`struct EnumerableSet.Bytes32Set`)
- **_DONE_TIMESTAMP** (`uint256`)
- **_timestamps** (`mapping(bytes32 => uint256)`)
- **_roles** (`mapping(bytes32 => struct AccessControl.RoleData)`)

## State Variable Writes

- **_operationsIdSet** (`struct EnumerableSet.Bytes32Set`)
- **_operationsMap** (`mapping(bytes32 => struct TimelockControllerEnumerable.Operation)`)
- **_operationsBatchIdSet** (`struct EnumerableSet.Bytes32Set`)
- **_operationsBatchMap** (`mapping(bytes32 => struct TimelockControllerEnumerable.OperationBatch)`)
- **_timestamps** (`mapping(bytes32 => uint256)`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: TimelockControllerEnumerable.cancel(bytes32) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
  ├─ [1] ⚙️ FUNCTION: TimelockController.cancel(bytes32) (NodeID: 1)
  │   💬 Args: [id]
  │   👁️  Def: public
  │ ├─ [2] ⚙️ FUNCTION: TimelockController.isOperationPending(bytes32) (NodeID: 2)
  │ │   💬 Args: [id]
  │ │   👁️  Def: public
  │ │ └─ [3] ⚙️ FUNCTION: TimelockController.getOperationState(bytes32) (NodeID: 3)
  │ │     💬 Args: [id]
  │ │     👁️  Def: public
  │ │   └─ [4] ⚙️ FUNCTION: TimelockController.getTimestamp(bytes32) (NodeID: 4)
  │ │       💬 Args: [id]
  │ │       👁️  Def: public
  │ ├─ [2] ⚙️ FUNCTION: TimelockController._encodeStateBitmap(enum TimelockController.OperationState) (NodeID: 5)
  │ │   💬 Args: [OperationState.Ready]
  │ │   👁️  Def: internal
  │ ├─ [2] ⚙️ FUNCTION: TimelockController._encodeStateBitmap(enum TimelockController.OperationState) (NodeID: 6)
  │ │   💬 Args: [OperationState.Waiting]
  │ │   👁️  Def: internal
  │ └─ [2] 🔒 MODIFIER: AccessControl.onlyRole(bytes32) (NodeID: 7)
  │     💬 Args: [CANCELLER_ROLE]
  │   └─ [3] ⚙️ FUNCTION: AccessControl._checkRole(bytes32) (NodeID: 8)
  │       💬 Args: [CANCELLER_ROLE]
  │       👁️  Def: internal
  │     └─ [4] ⚙️ FUNCTION: AccessControl._checkRole(bytes32,address) (NodeID: 9)
  │         💬 Args: [role, _msgSender()]
  │         👁️  Def: internal
  │       ├─ [5] ⚙️ FUNCTION: Context._msgSender() (NodeID: 11)
  │       │   💬 Args: [no args]
  │       │   👁️  Def: internal
  │       └─ [5] ⚙️ FUNCTION: AccessControl.hasRole(bytes32,address) (NodeID: 10)
  │           💬 Args: [role, account]
  │           👁️  Def: public
  ├─ [1] ⚙️ FUNCTION: EnumerableSet.contains(struct EnumerableSet.Bytes32Set,bytes32) (NodeID: 12)
  │   💬 Args: [_operationsIdSet, id]
  │   👁️  Def: internal
  │ └─ [2] ⚙️ FUNCTION: EnumerableSet._contains(struct EnumerableSet.Set,bytes32) (NodeID: 13)
  │     💬 Args: [set._inner, value]
  │     👁️  Def: private
  ├─ [1] ⚙️ FUNCTION: EnumerableSet.remove(struct EnumerableSet.Bytes32Set,bytes32) (NodeID: 14)
  │   💬 Args: [_operationsIdSet, id]
  │   👁️  Def: internal
  │ └─ [2] ⚙️ FUNCTION: EnumerableSet._remove(struct EnumerableSet.Set,bytes32) (NodeID: 15)
  │     💬 Args: [set._inner, value]
  │     👁️  Def: private
  ├─ [1] ⚙️ FUNCTION: EnumerableSet.contains(struct EnumerableSet.Bytes32Set,bytes32) (NodeID: 16)
  │   💬 Args: [_operationsBatchIdSet, id]
  │   👁️  Def: internal
  │ └─ [2] ⚙️ FUNCTION: EnumerableSet._contains(struct EnumerableSet.Set,bytes32) (NodeID: 17)
  │     💬 Args: [set._inner, value]
  │     👁️  Def: private
  └─ [1] ⚙️ FUNCTION: EnumerableSet.remove(struct EnumerableSet.Bytes32Set,bytes32) (NodeID: 18)
      💬 Args: [_operationsBatchIdSet, id]
      👁️  Def: internal
    └─ [2] ⚙️ FUNCTION: EnumerableSet._remove(struct EnumerableSet.Set,bytes32) (NodeID: 19)
        💬 Args: [set._inner, value]
        👁️  Def: private
```

## Documentation

### Function Documentation

@inheritdoc TimelockController
 @dev Remove the operation
