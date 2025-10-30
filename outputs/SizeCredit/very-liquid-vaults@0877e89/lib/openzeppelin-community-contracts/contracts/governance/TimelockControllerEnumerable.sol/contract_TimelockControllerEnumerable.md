# Contract: TimelockControllerEnumerable

## Metadata

- **Name**: TimelockControllerEnumerable
- **Type**: Contract
- **Path**: lib/openzeppelin-community-contracts/contracts/governance/TimelockControllerEnumerable.sol
- **Documentation**: @dev Extends the TimelockController to allow for enumerable operations

## Implements Interfaces

- **IERC1155Receiver** [lib/openzeppelin-contracts/contracts/token/ERC1155/IERC1155Receiver.sol/interface_IERC1155Receiver.md]
- **IERC721Receiver** [lib/openzeppelin-contracts/contracts/token/ERC721/IERC721Receiver.sol/interface_IERC721Receiver.md]
- **IERC165** [lib/openzeppelin-contracts/contracts/utils/introspection/IERC165.sol/interface_IERC165.md]
- **IAccessControl** [lib/openzeppelin-contracts/contracts/access/IAccessControl.sol/interface_IAccessControl.md]

## State Variables

### _roles (inherited from AccessControl)

```solidity
mapping(bytes32 => RoleData) private _roles
```

### DEFAULT_ADMIN_ROLE (inherited from AccessControl)

```solidity
bytes32 public constant DEFAULT_ADMIN_ROLE = 0x00
```

### PROPOSER_ROLE (inherited from TimelockController)

```solidity
bytes32 public constant PROPOSER_ROLE = keccak256("PROPOSER_ROLE")
```

### EXECUTOR_ROLE (inherited from TimelockController)

```solidity
bytes32 public constant EXECUTOR_ROLE = keccak256("EXECUTOR_ROLE")
```

### CANCELLER_ROLE (inherited from TimelockController)

```solidity
bytes32 public constant CANCELLER_ROLE = keccak256("CANCELLER_ROLE")
```

### _DONE_TIMESTAMP (inherited from TimelockController)

```solidity
uint256 internal constant _DONE_TIMESTAMP = uint256(1)
```

### _timestamps (inherited from TimelockController)

```solidity
mapping(bytes32 => uint256) private _timestamps
```

### _minDelay (inherited from TimelockController)

```solidity
uint256 private _minDelay
```

### _operationsIdSet

```solidity
/// @notice The operations id set
EnumerableSet.Bytes32Set private _operationsIdSet
```

### _operationsMap

```solidity
/// @notice The operations map
mapping(bytes32 => Operation) private _operationsMap
```

### _operationsBatchIdSet

```solidity
/// @notice The operations batch id set
EnumerableSet.Bytes32Set private _operationsBatchIdSet
```

### _operationsBatchMap

```solidity
/// @notice The operations batch map
mapping(bytes32 => OperationBatch) private _operationsBatchMap
```

## Structs

### RoleData (inherited from AccessControl)

```solidity
struct RoleData {
    mapping(address => bool) hasRole;
    bytes32 adminRole;
}
```

### Operation

```solidity
/// @notice The operation struct
struct Operation {
    address target;
    uint256 value;
    bytes data;
    bytes32 predecessor;
    bytes32 salt;
    uint256 delay;
}
```

### OperationBatch

```solidity
/// @notice The operation batch struct
struct OperationBatch {
    address[] targets;
    uint256[] values;
    bytes[] payloads;
    bytes32 predecessor;
    bytes32 salt;
    uint256 delay;
}
```

## Errors

### AccessControlUnauthorizedAccount (inherited from IAccessControl)

```solidity
///  @dev The `account` is missing a role.
error AccessControlUnauthorizedAccount(address account, bytes32 neededRole);
```

### AccessControlBadConfirmation (inherited from IAccessControl)

```solidity
///  @dev The caller of a function is not the expected one.
///  NOTE: Don't confuse with {AccessControlUnauthorizedAccount}.
error AccessControlBadConfirmation();
```

### TimelockInvalidOperationLength (inherited from TimelockController)

```solidity
///  @dev Mismatch between the parameters length for an operation call.
error TimelockInvalidOperationLength(uint256 targets, uint256 payloads, uint256 values);
```

### TimelockInsufficientDelay (inherited from TimelockController)

```solidity
///  @dev The schedule operation doesn't meet the minimum delay.
error TimelockInsufficientDelay(uint256 delay, uint256 minDelay);
```

### TimelockUnexpectedOperationState (inherited from TimelockController)

```solidity
///  @dev The current state of an operation is not as required.
///  The `expectedStates` is a bitmap with the bits enabled for each OperationState enum position
///  counting from right to left.
///  See {_encodeStateBitmap}.
error TimelockUnexpectedOperationState(bytes32 operationId, bytes32 expectedStates);
```

### TimelockUnexecutedPredecessor (inherited from TimelockController)

```solidity
///  @dev The predecessor to an operation not yet done.
error TimelockUnexecutedPredecessor(bytes32 predecessorId);
```

### TimelockUnauthorizedCaller (inherited from TimelockController)

```solidity
///  @dev The caller account is not authorized.
error TimelockUnauthorizedCaller(address caller);
```

### OperationIndexNotFound

```solidity
/// @notice The error when the operation index is not found
error OperationIndexNotFound(uint256 index);
```

### OperationIdNotFound

```solidity
/// @notice The error when the operation id is not found
error OperationIdNotFound(bytes32 id);
```

### OperationBatchIndexNotFound

```solidity
/// @notice The error when the operation batch index is not found
error OperationBatchIndexNotFound(uint256 index);
```

### OperationBatchIdNotFound

```solidity
/// @notice The error when the operation batch id is not found
error OperationBatchIdNotFound(bytes32 id);
```

## Events

### RoleAdminChanged (inherited from IAccessControl)

```solidity
///  @dev Emitted when `newAdminRole` is set as ``role``'s admin role, replacing `previousAdminRole`
///  `DEFAULT_ADMIN_ROLE` is the starting admin for all roles, despite
///  {RoleAdminChanged} not being emitted to signal this.
event RoleAdminChanged(bytes32 indexed role, bytes32 indexed previousAdminRole, bytes32 indexed newAdminRole);
```

### RoleGranted (inherited from IAccessControl)

```solidity
///  @dev Emitted when `account` is granted `role`.
///  `sender` is the account that originated the contract call. This account bears the admin role (for the granted role).
///  Expected in cases where the role was granted using the internal {AccessControl-_grantRole}.
event RoleGranted(bytes32 indexed role, address indexed account, address indexed sender);
```

### RoleRevoked (inherited from IAccessControl)

```solidity
///  @dev Emitted when `account` is revoked `role`.
///  `sender` is the account that originated the contract call:
///    - if using `revokeRole`, it is the admin role bearer
///    - if using `renounceRole`, it is the role bearer (i.e. `account`)
event RoleRevoked(bytes32 indexed role, address indexed account, address indexed sender);
```

### CallScheduled (inherited from TimelockController)

```solidity
///  @dev Emitted when a call is scheduled as part of operation `id`.
event CallScheduled(bytes32 indexed id, uint256 indexed index, address target, uint256 value, bytes data, bytes32 predecessor, uint256 delay);
```

### CallExecuted (inherited from TimelockController)

```solidity
///  @dev Emitted when a call is performed as part of operation `id`.
event CallExecuted(bytes32 indexed id, uint256 indexed index, address target, uint256 value, bytes data);
```

### CallSalt (inherited from TimelockController)

```solidity
///  @dev Emitted when new proposal is scheduled with non-zero salt.
event CallSalt(bytes32 indexed id, bytes32 salt);
```

### Cancelled (inherited from TimelockController)

```solidity
///  @dev Emitted when operation `id` is cancelled.
event Cancelled(bytes32 indexed id);
```

### MinDelayChange (inherited from TimelockController)

```solidity
///  @dev Emitted when the minimum delay for future operations is modified.
event MinDelayChange(uint256 oldDuration, uint256 newDuration);
```

## Enums

### OperationState (inherited from TimelockController)

```solidity
enum OperationState {
    Unset,
    Waiting,
    Ready,
    Done
}
```

## Public/External Functions

### constructor(uint256,address[],address[],address)

- **Signature**: `constructor(uint256,address[],address[],address)`
- **Visibility**: public
- **Source Range**: 1764:199:53
- **Details**: [function_constructor_uint256_address[]_address[]_address.md](./function_constructor_uint256_address[]_address[]_address.md)

**Signature:**
```solidity
constructor(uint256 minDelay, address[] memory proposers, address[] memory executors, address admin) TimelockController(minDelay,proposers,executors,admin);
```

### schedule(address,uint256,bytes,bytes32,bytes32,uint256)

- **Signature**: `schedule(address,uint256,bytes,bytes32,bytes32,uint256)`
- **Visibility**: public
- **Source Range**: 2041:604:53
- **Details**: [function_schedule_address_uint256_bytes_bytes32_bytes32_uint256.md](./function_schedule_address_uint256_bytes_bytes32_bytes32_uint256.md)

**Signature:**
```solidity
/// @inheritdoc TimelockController
///  @dev Store the operation
function schedule(address target, uint256 value, bytes calldata data, bytes32 predecessor, bytes32 salt, uint256 delay) virtual override public;
```

### scheduleBatch(address[],uint256[],bytes[],bytes32,bytes32,uint256)

- **Signature**: `scheduleBatch(address[],uint256[],bytes[],bytes32,bytes32,uint256)`
- **Visibility**: public
- **Source Range**: 2728:688:53
- **Details**: [function_scheduleBatch_address[]_uint256[]_bytes[]_bytes32_bytes32_uint256.md](./function_scheduleBatch_address[]_uint256[]_bytes[]_bytes32_bytes32_uint256.md)

**Signature:**
```solidity
/// @inheritdoc TimelockController
///  @dev Store the operationBatch
function scheduleBatch(address[] calldata targets, uint256[] calldata values, bytes[] calldata payloads, bytes32 predecessor, bytes32 salt, uint256 delay) virtual override public;
```

### cancel(bytes32)

- **Signature**: `cancel(bytes32)`
- **Visibility**: public
- **Source Range**: 3495:370:53
- **Details**: [function_cancel_bytes32.md](./function_cancel_bytes32.md)

**Signature:**
```solidity
/// @inheritdoc TimelockController
///  @dev Remove the operation
function cancel(bytes32 id) virtual override public;
```

### operations()

- **Signature**: `operations()`
- **Visibility**: public
- **Source Range**: 3955:365:53
- **Details**: [function_operations.md](./function_operations.md)

**Signature:**
```solidity
/// @dev Return the operations
///  @return operations_ The operations array
function operations() public view returns (Operation[] memory operations_);
```

### operationsCount()

- **Signature**: `operationsCount()`
- **Visibility**: public
- **Source Range**: 4442:168:53
- **Details**: [function_operationsCount.md](./function_operationsCount.md)

**Signature:**
```solidity
/// @dev Return the number of operations from the set
///  @return operationsCount_ The number of operations
function operationsCount() public view returns (uint256 operationsCount_);
```

### operation(uint256)

- **Signature**: `operation(uint256)`
- **Visibility**: public
- **Source Range**: 4758:293:53
- **Details**: [function_operation_uint256.md](./function_operation_uint256.md)

**Signature:**
```solidity
/// @dev Return the operation at the given index
///  @param index The index of the operation
///  @return operation_ The operation
function operation(uint256 index) public view returns (Operation memory operation_);
```

### operation(bytes32)

- **Signature**: `operation(bytes32)`
- **Visibility**: public
- **Source Range**: 5192:256:53
- **Details**: [function_operation_bytes32.md](./function_operation_bytes32.md)

**Signature:**
```solidity
/// @dev Return the operation with the given id
///  @param id The id of the operation
///  @return operation_ The operation
function operation(bytes32 id) public view returns (Operation memory operation_);
```

### operationsBatch()

- **Signature**: `operationsBatch()`
- **Visibility**: public
- **Source Range**: 5553:430:53
- **Details**: [function_operationsBatch.md](./function_operationsBatch.md)

**Signature:**
```solidity
/// @dev Return the operationsBatch
///  @return operationsBatch_ The operationsBatch array
function operationsBatch() public view returns (OperationBatch[] memory operationsBatch_);
```

### operationsBatchCount()

- **Signature**: `operationsBatchCount()`
- **Visibility**: public
- **Source Range**: 6120:193:53
- **Details**: [function_operationsBatchCount.md](./function_operationsBatchCount.md)

**Signature:**
```solidity
/// @dev Return the number of operationsBatch from the set
///  @return operationsBatchCount_ The number of operationsBatch
function operationsBatchCount() public view returns (uint256 operationsBatchCount_);
```

### operationBatch(uint256)

- **Signature**: `operationBatch(uint256)`
- **Visibility**: public
- **Source Range**: 6484:338:53
- **Details**: [function_operationBatch_uint256.md](./function_operationBatch_uint256.md)

**Signature:**
```solidity
/// @dev Return the operationsBatch at the given index
///  @param index The index of the operationsBatch
///  @return operationBatch_ The operationsBatch
function operationBatch(uint256 index) public view returns (OperationBatch memory operationBatch_);
```

### operationBatch(bytes32)

- **Signature**: `operationBatch(bytes32)`
- **Visibility**: public
- **Source Range**: 6986:296:53
- **Details**: [function_operationBatch_bytes32.md](./function_operationBatch_bytes32.md)

**Signature:**
```solidity
/// @dev Return the operationsBatch with the given id
///  @param id The id of the operationsBatch
///  @return operationBatch_ The operationsBatch
function operationBatch(bytes32 id) public view returns (OperationBatch memory operationBatch_);
```

### supportsInterface(bytes4) (inherited from ERC165)

- **Signature**: `supportsInterface(bytes4)`
- **Visibility**: public
- **Source Range**: 763:146:107
- **Details**: [function_supportsInterface_bytes4.md](./function_supportsInterface_bytes4.md)

**Signature:**
```solidity
///  @dev See {IERC165-supportsInterface}.
function supportsInterface(bytes4 interfaceId) virtual public view returns (bool);
```

### hasRole(bytes32,address) (inherited from AccessControl)

- **Signature**: `hasRole(bytes32,address)`
- **Visibility**: public
- **Source Range**: 2854:136:68
- **Details**: [function_hasRole_bytes32_address.md](./function_hasRole_bytes32_address.md)

**Signature:**
```solidity
///  @dev Returns `true` if `account` has been granted `role`.
function hasRole(bytes32 role, address account) virtual public view returns (bool);
```

### getRoleAdmin(bytes32) (inherited from AccessControl)

- **Signature**: `getRoleAdmin(bytes32)`
- **Visibility**: public
- **Source Range**: 3810:120:68
- **Details**: [function_getRoleAdmin_bytes32.md](./function_getRoleAdmin_bytes32.md)

**Signature:**
```solidity
///  @dev Returns the admin role that controls `role`. See {grantRole} and
///  {revokeRole}.
///  To change a role's admin, use {_setRoleAdmin}.
function getRoleAdmin(bytes32 role) virtual public view returns (bytes32);
```

### grantRole(bytes32,address) (inherited from AccessControl)

- **Signature**: `grantRole(bytes32,address)`
- **Visibility**: public
- **Source Range**: 4226:136:68
- **Details**: [function_grantRole_bytes32_address.md](./function_grantRole_bytes32_address.md)

**Signature:**
```solidity
///  @dev Grants `role` to `account`.
///  If `account` had not been already granted `role`, emits a {RoleGranted}
///  event.
///  Requirements:
///  - the caller must have ``role``'s admin role.
///  May emit a {RoleGranted} event.
function grantRole(bytes32 role, address account) virtual public onlyRole(getRoleAdmin(role));
```

### revokeRole(bytes32,address) (inherited from AccessControl)

- **Signature**: `revokeRole(bytes32,address)`
- **Visibility**: public
- **Source Range**: 4642:138:68
- **Details**: [function_revokeRole_bytes32_address.md](./function_revokeRole_bytes32_address.md)

**Signature:**
```solidity
///  @dev Revokes `role` from `account`.
///  If `account` had been granted `role`, emits a {RoleRevoked} event.
///  Requirements:
///  - the caller must have ``role``'s admin role.
///  May emit a {RoleRevoked} event.
function revokeRole(bytes32 role, address account) virtual public onlyRole(getRoleAdmin(role));
```

### renounceRole(bytes32,address) (inherited from AccessControl)

- **Signature**: `renounceRole(bytes32,address)`
- **Visibility**: public
- **Source Range**: 5328:245:68
- **Details**: [function_renounceRole_bytes32_address.md](./function_renounceRole_bytes32_address.md)

**Signature:**
```solidity
///  @dev Revokes `role` from the calling account.
///  Roles are often managed via {grantRole} and {revokeRole}: this function's
///  purpose is to provide a mechanism for accounts to lose their privileges
///  if they are compromised (such as when a trusted device is misplaced).
///  If the calling account had been revoked `role`, emits a {RoleRevoked}
///  event.
///  Requirements:
///  - the caller must be `callerConfirmation`.
///  May emit a {RoleRevoked} event.
function renounceRole(bytes32 role, address callerConfirmation) virtual public;
```

### onERC721Received(address,address,uint256,bytes) (inherited from ERC721Holder)

- **Signature**: `onERC721Received(address,address,uint256,bytes)`
- **Visibility**: public
- **Source Range**: 639:153:94
- **Details**: [function_onERC721Received_address_address_uint256_bytes.md](./function_onERC721Received_address_address_uint256_bytes.md)

**Signature:**
```solidity
///  @dev See {IERC721Receiver-onERC721Received}.
///  Always returns `IERC721Receiver.onERC721Received.selector`.
function onERC721Received(address, address, uint256, bytes memory) virtual public returns (bytes4);
```

### onERC1155Received(address,address,uint256,uint256,bytes) (inherited from ERC1155Holder)

- **Signature**: `onERC1155Received(address,address,uint256,uint256,bytes)`
- **Visibility**: public
- **Source Range**: 877:219:86
- **Details**: [function_onERC1155Received_address_address_uint256_uint256_bytes.md](./function_onERC1155Received_address_address_uint256_uint256_bytes.md)

**Signature:**
```solidity
function onERC1155Received(address, address, uint256, uint256, bytes memory) virtual override public returns (bytes4);
```

### onERC1155BatchReceived(address,address,uint256[],uint256[],bytes) (inherited from ERC1155Holder)

- **Signature**: `onERC1155BatchReceived(address,address,uint256[],uint256[],bytes)`
- **Visibility**: public
- **Source Range**: 1102:247:86
- **Details**: [function_onERC1155BatchReceived_address_address_uint256[]_uint256[]_bytes.md](./function_onERC1155BatchReceived_address_address_uint256[]_uint256[]_bytes.md)

**Signature:**
```solidity
function onERC1155BatchReceived(address, address, uint256[] memory, uint256[] memory, bytes memory) virtual override public returns (bytes4);
```

### receive() (inherited from TimelockController)

- **Signature**: `receive()`
- **Visibility**: external
- **Source Range**: 5549:37:72
- **Details**: [function_receive.md](./function_receive.md)

**Signature:**
```solidity
///  @dev Contract might receive/hold ETH as part of the maintenance process.
receive() virtual external payable;
```

### isOperation(bytes32) (inherited from TimelockController)

- **Signature**: `isOperation(bytes32)`
- **Visibility**: public
- **Source Range**: 6006:129:72
- **Details**: [function_isOperation_bytes32.md](./function_isOperation_bytes32.md)

**Signature:**
```solidity
///  @dev Returns whether an id corresponds to a registered operation. This
///  includes both Waiting, Ready, and Done operations.
function isOperation(bytes32 id) public view returns (bool);
```

### isOperationPending(bytes32) (inherited from TimelockController)

- **Signature**: `isOperationPending(bytes32)`
- **Visibility**: public
- **Source Range**: 6270:209:72
- **Details**: [function_isOperationPending_bytes32.md](./function_isOperationPending_bytes32.md)

**Signature:**
```solidity
///  @dev Returns whether an operation is pending or not. Note that a "pending" operation may also be "ready".
function isOperationPending(bytes32 id) public view returns (bool);
```

### isOperationReady(bytes32) (inherited from TimelockController)

- **Signature**: `isOperationReady(bytes32)`
- **Visibility**: public
- **Source Range**: 6615:134:72
- **Details**: [function_isOperationReady_bytes32.md](./function_isOperationReady_bytes32.md)

**Signature:**
```solidity
///  @dev Returns whether an operation is ready for execution. Note that a "ready" operation is also "pending".
function isOperationReady(bytes32 id) public view returns (bool);
```

### isOperationDone(bytes32) (inherited from TimelockController)

- **Signature**: `isOperationDone(bytes32)`
- **Visibility**: public
- **Source Range**: 6828:132:72
- **Details**: [function_isOperationDone_bytes32.md](./function_isOperationDone_bytes32.md)

**Signature:**
```solidity
///  @dev Returns whether an operation is done or not.
function isOperationDone(bytes32 id) public view returns (bool);
```

### getTimestamp(bytes32) (inherited from TimelockController)

- **Signature**: `getTimestamp(bytes32)`
- **Visibility**: public
- **Source Range**: 7108:111:72
- **Details**: [function_getTimestamp_bytes32.md](./function_getTimestamp_bytes32.md)

**Signature:**
```solidity
///  @dev Returns the timestamp at which an operation becomes ready (0 for
///  unset operations, 1 for done operations).
function getTimestamp(bytes32 id) virtual public view returns (uint256);
```

### getOperationState(bytes32) (inherited from TimelockController)

- **Signature**: `getOperationState(bytes32)`
- **Visibility**: public
- **Source Range**: 7278:460:72
- **Details**: [function_getOperationState_bytes32.md](./function_getOperationState_bytes32.md)

**Signature:**
```solidity
///  @dev Returns operation state.
function getOperationState(bytes32 id) virtual public view returns (OperationState);
```

### getMinDelay() (inherited from TimelockController)

- **Signature**: `getMinDelay()`
- **Visibility**: public
- **Source Range**: 7935:94:72
- **Details**: [function_getMinDelay.md](./function_getMinDelay.md)

**Signature:**
```solidity
///  @dev Returns the minimum delay in seconds for an operation to become valid.
///  This value can be changed by executing an operation that calls `updateDelay`.
function getMinDelay() virtual public view returns (uint256);
```

### hashOperation(address,uint256,bytes,bytes32,bytes32) (inherited from TimelockController)

- **Signature**: `hashOperation(address,uint256,bytes,bytes32,bytes32)`
- **Visibility**: public
- **Source Range**: 8142:279:72
- **Details**: [function_hashOperation_address_uint256_bytes_bytes32_bytes32.md](./function_hashOperation_address_uint256_bytes_bytes32_bytes32.md)

**Signature:**
```solidity
///  @dev Returns the identifier of an operation containing a single
///  transaction.
function hashOperation(address target, uint256 value, bytes calldata data, bytes32 predecessor, bytes32 salt) virtual public pure returns (bytes32);
```

### hashOperationBatch(address[],uint256[],bytes[],bytes32,bytes32) (inherited from TimelockController)

- **Signature**: `hashOperationBatch(address[],uint256[],bytes[],bytes32,bytes32)`
- **Visibility**: public
- **Source Range**: 8537:320:72
- **Details**: [function_hashOperationBatch_address[]_uint256[]_bytes[]_bytes32_bytes32.md](./function_hashOperationBatch_address[]_uint256[]_bytes[]_bytes32_bytes32.md)

**Signature:**
```solidity
///  @dev Returns the identifier of an operation containing a batch of
///  transactions.
function hashOperationBatch(address[] calldata targets, uint256[] calldata values, bytes[] calldata payloads, bytes32 predecessor, bytes32 salt) virtual public pure returns (bytes32);
```

### execute(address,uint256,bytes,bytes32,bytes32) (inherited from TimelockController)

- **Signature**: `execute(address,uint256,bytes,bytes32,bytes32)`
- **Visibility**: public
- **Source Range**: 12174:459:72
- **Details**: [function_execute_address_uint256_bytes_bytes32_bytes32.md](./function_execute_address_uint256_bytes_bytes32_bytes32.md)

**Signature:**
```solidity
///  @dev Execute an (ready) operation containing a single transaction.
///  Emits a {CallExecuted} event.
///  Requirements:
///  - the caller must have the 'executor' role.
function execute(address target, uint256 value, bytes calldata payload, bytes32 predecessor, bytes32 salt) virtual public payable onlyRoleOrOpenRole(EXECUTOR_ROLE);
```

### executeBatch(address[],uint256[],bytes[],bytes32,bytes32) (inherited from TimelockController)

- **Signature**: `executeBatch(address[],uint256[],bytes[],bytes32,bytes32)`
- **Visibility**: public
- **Source Range**: 13141:896:72
- **Details**: [function_executeBatch_address[]_uint256[]_bytes[]_bytes32_bytes32.md](./function_executeBatch_address[]_uint256[]_bytes[]_bytes32_bytes32.md)

**Signature:**
```solidity
///  @dev Execute an (ready) operation containing a batch of transactions.
///  Emits one {CallExecuted} event per transaction in the batch.
///  Requirements:
///  - the caller must have the 'executor' role.
function executeBatch(address[] calldata targets, uint256[] calldata values, bytes[] calldata payloads, bytes32 predecessor, bytes32 salt) virtual public payable onlyRoleOrOpenRole(EXECUTOR_ROLE);
```

### updateDelay(uint256) (inherited from TimelockController)

- **Signature**: `updateDelay(uint256)`
- **Visibility**: external
- **Source Range**: 15493:286:72
- **Details**: [function_updateDelay_uint256.md](./function_updateDelay_uint256.md)

**Signature:**
```solidity
///  @dev Changes the minimum timelock duration for future operations.
///  Emits a {MinDelayChange} event.
///  Requirements:
///  - the caller must be the timelock itself. This can only be achieved by scheduling and later executing
///  an operation where the timelock is the target and the data is the ABI-encoded call to this function.
function updateDelay(uint256 newDelay) virtual external;
```
