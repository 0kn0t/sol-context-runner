# Function: scheduleBatch(address[],uint256[],bytes[],bytes32,bytes32,uint256)

**Contract**: [lib/openzeppelin-community-contracts/contracts/governance/TimelockControllerEnumerable.sol/contract_TimelockControllerEnumerable.md]

## Metadata

- **Contract**: TimelockControllerEnumerable
- **Signature**: `scheduleBatch(address[],uint256[],bytes[],bytes32,bytes32,uint256)`
- **Visibility**: public
- **Source Range**: 2728:688:53

## Implementation

```solidity
/// @inheritdoc TimelockController
///  @dev Store the operationBatch
function scheduleBatch(address[] calldata targets, uint256[] calldata values, bytes[] calldata payloads, bytes32 predecessor, bytes32 salt, uint256 delay) virtual override public {
    super.scheduleBatch(targets, values, payloads, predecessor, salt, delay);
    bytes32 id = hashOperationBatch(targets, values, payloads, predecessor, salt);
    _operationsBatchIdSet.add(id);
    _operationsBatchMap[id] = OperationBatch({targets: targets, values: values, payloads: payloads, predecessor: predecessor, salt: salt, delay: delay});
}
```

## Related Implementations

### scheduleBatch(address[],uint256[],bytes[],bytes32,bytes32,uint256)

- **Kind**: internal
- **Source**: 9876:807:72
- **Link**: `lib/openzeppelin-contracts/contracts/governance/TimelockController.sol:TimelockController:scheduleBatch(address[],uint256[],bytes[],bytes32,bytes32,uint256)`

```solidity
///  @dev Schedule an operation containing a batch of transactions.
///  Emits {CallSalt} if salt is nonzero, and one {CallScheduled} event per transaction in the batch.
///  Requirements:
///  - the caller must have the 'proposer' role.
function scheduleBatch(address[] calldata targets, uint256[] calldata values, bytes[] calldata payloads, bytes32 predecessor, bytes32 salt, uint256 delay) virtual public onlyRole(PROPOSER_ROLE) {
    if ((targets.length != values.length) || (targets.length != payloads.length)) {
        revert TimelockInvalidOperationLength(targets.length, payloads.length, values.length);
    }
    bytes32 id = hashOperationBatch(targets, values, payloads, predecessor, salt);
    _schedule(id, delay);
    for (uint256 i = 0; i < targets.length; ++i) {
        emit CallScheduled(id, i, targets[i], values[i], payloads[i], predecessor, delay);
    }
    if (salt != bytes32(0)) {
        emit CallSalt(id, salt);
    }
}
```

### hashOperationBatch(address[],uint256[],bytes[],bytes32,bytes32)

- **Kind**: internal
- **Source**: 8537:320:72
- **Link**: `lib/openzeppelin-contracts/contracts/governance/TimelockController.sol:TimelockController:hashOperationBatch(address[],uint256[],bytes[],bytes32,bytes32)`

```solidity
///  @dev Returns the identifier of an operation containing a batch of
///  transactions.
function hashOperationBatch(address[] calldata targets, uint256[] calldata values, bytes[] calldata payloads, bytes32 predecessor, bytes32 salt) virtual public pure returns (bytes32) {
    return keccak256(abi.encode(targets, values, payloads, predecessor, salt));
}
```

### _schedule(bytes32,uint256)

- **Kind**: internal
- **Source**: 10784:399:72
- **Link**: `lib/openzeppelin-contracts/contracts/governance/TimelockController.sol:TimelockController:_schedule(bytes32,uint256)`

```solidity
///  @dev Schedule an operation that is to become valid after a given delay.
function _schedule(bytes32 id, uint256 delay) private {
    if (isOperation(id)) {
        revert TimelockUnexpectedOperationState(id, _encodeStateBitmap(OperationState.Unset));
    }
    uint256 minDelay = getMinDelay();
    if (delay < minDelay) {
        revert TimelockInsufficientDelay(delay, minDelay);
    }
    _timestamps[id] = block.timestamp + delay;
}
```

### isOperation(bytes32)

- **Kind**: internal
- **Source**: 6006:129:72
- **Link**: `lib/openzeppelin-contracts/contracts/governance/TimelockController.sol:TimelockController:isOperation(bytes32)`

```solidity
///  @dev Returns whether an id corresponds to a registered operation. This
///  includes both Waiting, Ready, and Done operations.
function isOperation(bytes32 id) public view returns (bool) {
    return getOperationState(id) != OperationState.Unset;
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

### getMinDelay()

- **Kind**: internal
- **Source**: 7935:94:72
- **Link**: `lib/openzeppelin-contracts/contracts/governance/TimelockController.sol:TimelockController:getMinDelay()`

```solidity
///  @dev Returns the minimum delay in seconds for an operation to become valid.
///  This value can be changed by executing an operation that calls `updateDelay`.
function getMinDelay() virtual public view returns (uint256) {
    return _minDelay;
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

### add(struct EnumerableSet.Bytes32Set,bytes32)

- **Kind**: internal
- **Source**: 6576:123:112
- **Link**: `lib/openzeppelin-contracts/contracts/utils/structs/EnumerableSet.sol:EnumerableSet:add(struct EnumerableSet.Bytes32Set,bytes32)`

```solidity
///  @dev Add a value to a set. O(1).
///  Returns true if the value was added to the set, that is if it was not
///  already present.
function add(Bytes32Set storage set, bytes32 value) internal returns (bool) {
    return _add(set._inner, value);
}
```

### _add(struct EnumerableSet.Set,bytes32)

- **Kind**: internal
- **Source**: 2336:406:112
- **Link**: `lib/openzeppelin-contracts/contracts/utils/structs/EnumerableSet.sol:EnumerableSet:_add(struct EnumerableSet.Set,bytes32)`

```solidity
///  @dev Add a value to a set. O(1).
///  Returns true if the value was added to the set, that is if it was not
///  already present.
function _add(Set storage set, bytes32 value) private returns (bool) {
    if (!_contains(set, value)) {
        set._values.push(value);
        set._positions[value] = set._values.length;
        return true;
    } else {
        return false;
    }
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

## State Variable Reads

- **_DONE_TIMESTAMP** (`uint256`)
- **_timestamps** (`mapping(bytes32 => uint256)`)
- **_minDelay** (`uint256`)
- **_roles** (`mapping(bytes32 => struct AccessControl.RoleData)`)

## State Variable Writes

- **_operationsBatchIdSet** (`struct EnumerableSet.Bytes32Set`)
- **_operationsBatchMap** (`mapping(bytes32 => struct TimelockControllerEnumerable.OperationBatch)`)
- **_timestamps** (`mapping(bytes32 => uint256)`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: TimelockControllerEnumerable.scheduleBatch(address[],uint256[],bytes[],bytes32,bytes32,uint256) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
  ├─ [1] ⚙️ FUNCTION: TimelockController.scheduleBatch(address[],uint256[],bytes[],bytes32,bytes32,uint256) (NodeID: 1)
  │   💬 Args: [targets, values, payloads, predecessor, salt, delay]
  │   👁️  Def: public
  │ ├─ [2] ⚙️ FUNCTION: TimelockController.hashOperationBatch(address[],uint256[],bytes[],bytes32,bytes32) (NodeID: 2)
  │ │   💬 Args: [targets, values, payloads, predecessor, salt]
  │ │   👁️  Def: public
  │ ├─ [2] ⚙️ FUNCTION: TimelockController._schedule(bytes32,uint256) (NodeID: 3)
  │ │   💬 Args: [id, delay]
  │ │   👁️  Def: private
  │ │ ├─ [3] ⚙️ FUNCTION: TimelockController.isOperation(bytes32) (NodeID: 4)
  │ │ │   💬 Args: [id]
  │ │ │   👁️  Def: public
  │ │ │ └─ [4] ⚙️ FUNCTION: TimelockController.getOperationState(bytes32) (NodeID: 5)
  │ │ │     💬 Args: [id]
  │ │ │     👁️  Def: public
  │ │ │   └─ [5] ⚙️ FUNCTION: TimelockController.getTimestamp(bytes32) (NodeID: 6)
  │ │ │       💬 Args: [id]
  │ │ │       👁️  Def: public
  │ │ ├─ [3] ⚙️ FUNCTION: TimelockController._encodeStateBitmap(enum TimelockController.OperationState) (NodeID: 7)
  │ │ │   💬 Args: [OperationState.Unset]
  │ │ │   👁️  Def: internal
  │ │ └─ [3] ⚙️ FUNCTION: TimelockController.getMinDelay() (NodeID: 8)
  │ │     💬 Args: [no args]
  │ │     👁️  Def: public
  │ └─ [2] 🔒 MODIFIER: AccessControl.onlyRole(bytes32) (NodeID: 9)
  │     💬 Args: [PROPOSER_ROLE]
  │   └─ [3] ⚙️ FUNCTION: AccessControl._checkRole(bytes32) (NodeID: 10)
  │       💬 Args: [PROPOSER_ROLE]
  │       👁️  Def: internal
  │     └─ [4] ⚙️ FUNCTION: AccessControl._checkRole(bytes32,address) (NodeID: 11)
  │         💬 Args: [role, _msgSender()]
  │         👁️  Def: internal
  │       ├─ [5] ⚙️ FUNCTION: Context._msgSender() (NodeID: 13)
  │       │   💬 Args: [no args]
  │       │   👁️  Def: internal
  │       └─ [5] ⚙️ FUNCTION: AccessControl.hasRole(bytes32,address) (NodeID: 12)
  │           💬 Args: [role, account]
  │           👁️  Def: public
  ├─ [1] ⚙️ FUNCTION: TimelockController.hashOperationBatch(address[],uint256[],bytes[],bytes32,bytes32) (NodeID: 14)
  │   💬 Args: [targets, values, payloads, predecessor, salt]
  │   👁️  Def: public
  └─ [1] ⚙️ FUNCTION: EnumerableSet.add(struct EnumerableSet.Bytes32Set,bytes32) (NodeID: 15)
      💬 Args: [_operationsBatchIdSet, id]
      👁️  Def: internal
    └─ [2] ⚙️ FUNCTION: EnumerableSet._add(struct EnumerableSet.Set,bytes32) (NodeID: 16)
        💬 Args: [set._inner, value]
        👁️  Def: private
      └─ [3] ⚙️ FUNCTION: EnumerableSet._contains(struct EnumerableSet.Set,bytes32) (NodeID: 17)
          💬 Args: [set, value]
          👁️  Def: private
```

## Documentation

### Function Documentation

@inheritdoc TimelockController
 @dev Store the operationBatch
