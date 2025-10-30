# Contract: TimelockController

## Metadata

- **Name**: TimelockController
- **Type**: Contract
- **Path**: lib/openzeppelin-contracts/contracts/governance/TimelockController.sol
- **Documentation**:  @dev Contract module which acts as a timelocked controller. When set as the
   owner of an `Ownable` smart contract, it enforces a timelock on all
   `onlyOwner` maintenance operations. This gives time for users of the
   controlled contract to exit before a potentially dangerous maintenance
   operation is applied.
   By default, this contract is self administered, meaning administration tasks
   have to go through the timelock process. The proposer (resp executor) role
   is in charge of proposing (resp executing) operations. A common use case is
   to position this {TimelockController} as the owner of a smart contract, with
   a multisig or a DAO as the sole proposer.

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

### PROPOSER_ROLE

```solidity
bytes32 public constant PROPOSER_ROLE = keccak256("PROPOSER_ROLE")
```

### EXECUTOR_ROLE

```solidity
bytes32 public constant EXECUTOR_ROLE = keccak256("EXECUTOR_ROLE")
```

### CANCELLER_ROLE

```solidity
bytes32 public constant CANCELLER_ROLE = keccak256("CANCELLER_ROLE")
```

### _DONE_TIMESTAMP

```solidity
uint256 internal constant _DONE_TIMESTAMP = uint256(1)
```

### _timestamps

```solidity
mapping(bytes32 => uint256) private _timestamps
```

### _minDelay

```solidity
uint256 private _minDelay
```

## Structs

### RoleData (inherited from AccessControl)

```solidity
struct RoleData {
    mapping(address => bool) hasRole;
    bytes32 adminRole;
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

### TimelockInvalidOperationLength

```solidity
///  @dev Mismatch between the parameters length for an operation call.
error TimelockInvalidOperationLength(uint256 targets, uint256 payloads, uint256 values);
```

### TimelockInsufficientDelay

```solidity
///  @dev The schedule operation doesn't meet the minimum delay.
error TimelockInsufficientDelay(uint256 delay, uint256 minDelay);
```

### TimelockUnexpectedOperationState

```solidity
///  @dev The current state of an operation is not as required.
///  The `expectedStates` is a bitmap with the bits enabled for each OperationState enum position
///  counting from right to left.
///  See {_encodeStateBitmap}.
error TimelockUnexpectedOperationState(bytes32 operationId, bytes32 expectedStates);
```

### TimelockUnexecutedPredecessor

```solidity
///  @dev The predecessor to an operation not yet done.
error TimelockUnexecutedPredecessor(bytes32 predecessorId);
```

### TimelockUnauthorizedCaller

```solidity
///  @dev The caller account is not authorized.
error TimelockUnauthorizedCaller(address caller);
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

### CallScheduled

```solidity
///  @dev Emitted when a call is scheduled as part of operation `id`.
event CallScheduled(bytes32 indexed id, uint256 indexed index, address target, uint256 value, bytes data, bytes32 predecessor, uint256 delay);
```

### CallExecuted

```solidity
///  @dev Emitted when a call is performed as part of operation `id`.
event CallExecuted(bytes32 indexed id, uint256 indexed index, address target, uint256 value, bytes data);
```

### CallSalt

```solidity
///  @dev Emitted when new proposal is scheduled with non-zero salt.
event CallSalt(bytes32 indexed id, bytes32 salt);
```

### Cancelled

```solidity
///  @dev Emitted when operation `id` is cancelled.
event Cancelled(bytes32 indexed id);
```

### MinDelayChange

```solidity
///  @dev Emitted when the minimum delay for future operations is modified.
event MinDelayChange(uint256 oldDuration, uint256 newDuration);
```

## Enums

### OperationState

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
- **Source Range**: 4248:761:72
- **Details**: [function_constructor_uint256_address[]_address[]_address.md](./function_constructor_uint256_address[]_address[]_address.md)

**Signature:**
```solidity
///  @dev Initializes the contract with the following parameters:
///  - `minDelay`: initial minimum delay in seconds for operations
///  - `proposers`: accounts to be granted proposer and canceller roles
///  - `executors`: accounts to be granted executor role
///  - `admin`: optional account to be granted admin role; disable with zero address
///  IMPORTANT: The optional admin can aid with initial configuration of roles after deployment
///  without being subject to delay, but this role should be subsequently renounced in favor of
///  administration through timelocked proposals. Previous versions of this contract would assign
///  this admin to the deployer automatically and should be renounced as well.
constructor(uint256 minDelay, address[] memory proposers, address[] memory executors, address admin);
```

### receive()

- **Signature**: `receive()`
- **Visibility**: external
- **Source Range**: 5549:37:72
- **Details**: [function_receive.md](./function_receive.md)

**Signature:**
```solidity
///  @dev Contract might receive/hold ETH as part of the maintenance process.
receive() virtual external payable;
```

### supportsInterface(bytes4)

- **Signature**: `supportsInterface(bytes4)`
- **Visibility**: public
- **Source Range**: 5653:195:72
- **Details**: [function_supportsInterface_bytes4.md](./function_supportsInterface_bytes4.md)

**Signature:**
```solidity
///  @dev See {IERC165-supportsInterface}.
function supportsInterface(bytes4 interfaceId) virtual override(AccessControl, ERC1155Holder) public view returns (bool);
```

### isOperation(bytes32)

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

### isOperationPending(bytes32)

- **Signature**: `isOperationPending(bytes32)`
- **Visibility**: public
- **Source Range**: 6270:209:72
- **Details**: [function_isOperationPending_bytes32.md](./function_isOperationPending_bytes32.md)

**Signature:**
```solidity
///  @dev Returns whether an operation is pending or not. Note that a "pending" operation may also be "ready".
function isOperationPending(bytes32 id) public view returns (bool);
```

### isOperationReady(bytes32)

- **Signature**: `isOperationReady(bytes32)`
- **Visibility**: public
- **Source Range**: 6615:134:72
- **Details**: [function_isOperationReady_bytes32.md](./function_isOperationReady_bytes32.md)

**Signature:**
```solidity
///  @dev Returns whether an operation is ready for execution. Note that a "ready" operation is also "pending".
function isOperationReady(bytes32 id) public view returns (bool);
```

### isOperationDone(bytes32)

- **Signature**: `isOperationDone(bytes32)`
- **Visibility**: public
- **Source Range**: 6828:132:72
- **Details**: [function_isOperationDone_bytes32.md](./function_isOperationDone_bytes32.md)

**Signature:**
```solidity
///  @dev Returns whether an operation is done or not.
function isOperationDone(bytes32 id) public view returns (bool);
```

### getTimestamp(bytes32)

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

### getOperationState(bytes32)

- **Signature**: `getOperationState(bytes32)`
- **Visibility**: public
- **Source Range**: 7278:460:72
- **Details**: [function_getOperationState_bytes32.md](./function_getOperationState_bytes32.md)

**Signature:**
```solidity
///  @dev Returns operation state.
function getOperationState(bytes32 id) virtual public view returns (OperationState);
```

### getMinDelay()

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

### hashOperation(address,uint256,bytes,bytes32,bytes32)

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

### hashOperationBatch(address[],uint256[],bytes[],bytes32,bytes32)

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

### schedule(address,uint256,bytes,bytes32,bytes32,uint256)

- **Signature**: `schedule(address,uint256,bytes,bytes32,bytes32,uint256)`
- **Visibility**: public
- **Source Range**: 9104:483:72
- **Details**: [function_schedule_address_uint256_bytes_bytes32_bytes32_uint256.md](./function_schedule_address_uint256_bytes_bytes32_bytes32_uint256.md)

**Signature:**
```solidity
///  @dev Schedule an operation containing a single transaction.
///  Emits {CallSalt} if salt is nonzero, and {CallScheduled}.
///  Requirements:
///  - the caller must have the 'proposer' role.
function schedule(address target, uint256 value, bytes calldata data, bytes32 predecessor, bytes32 salt, uint256 delay) virtual public onlyRole(PROPOSER_ROLE);
```

### scheduleBatch(address[],uint256[],bytes[],bytes32,bytes32,uint256)

- **Signature**: `scheduleBatch(address[],uint256[],bytes[],bytes32,bytes32,uint256)`
- **Visibility**: public
- **Source Range**: 9876:807:72
- **Details**: [function_scheduleBatch_address[]_uint256[]_bytes[]_bytes32_bytes32_uint256.md](./function_scheduleBatch_address[]_uint256[]_bytes[]_bytes32_bytes32_uint256.md)

**Signature:**
```solidity
///  @dev Schedule an operation containing a batch of transactions.
///  Emits {CallSalt} if salt is nonzero, and one {CallScheduled} event per transaction in the batch.
///  Requirements:
///  - the caller must have the 'proposer' role.
function scheduleBatch(address[] calldata targets, uint256[] calldata values, bytes[] calldata payloads, bytes32 predecessor, bytes32 salt, uint256 delay) virtual public onlyRole(PROPOSER_ROLE);
```

### cancel(bytes32)

- **Signature**: `cancel(bytes32)`
- **Visibility**: public
- **Source Range**: 11325:375:72
- **Details**: [function_cancel_bytes32.md](./function_cancel_bytes32.md)

**Signature:**
```solidity
///  @dev Cancel an operation.
///  Requirements:
///  - the caller must have the 'canceller' role.
function cancel(bytes32 id) virtual public onlyRole(CANCELLER_ROLE);
```

### execute(address,uint256,bytes,bytes32,bytes32)

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

### executeBatch(address[],uint256[],bytes[],bytes32,bytes32)

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

### updateDelay(uint256)

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
