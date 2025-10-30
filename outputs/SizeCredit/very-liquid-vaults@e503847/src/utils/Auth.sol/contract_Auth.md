# Contract: Auth

## Metadata

- **Name**: Auth
- **Type**: Contract
- **Path**: src/utils/Auth.sol
- **Documentation**: @title Auth
   @custom:security-contact security@size.credit
   @author Size (https://size.credit/)
   @notice Authority acccess control contract with global pause functionality for the Size Meta Vault system

## Implements Interfaces

- **IERC165** [lib/openzeppelin-contracts/contracts/utils/introspection/IERC165.sol/interface_IERC165.md]
- **IAccessControlEnumerable** [lib/openzeppelin-contracts/contracts/access/extensions/IAccessControlEnumerable.sol/interface_IAccessControlEnumerable.md]
- **IAccessControl** [lib/openzeppelin-contracts/contracts/access/IAccessControl.sol/interface_IAccessControl.md]
- **IERC1822Proxiable** [lib/openzeppelin-contracts/contracts/interfaces/draft-IERC1822.sol/interface_IERC1822Proxiable.md]

## State Variables

### INITIALIZABLE_STORAGE (inherited from Initializable)

```solidity
bytes32 private constant INITIALIZABLE_STORAGE = 0xf0c57e16840df040f15088dc2f81fe391c3923bec73e23a9662efc9c229c6a00
```

### __self (inherited from UUPSUpgradeable)

```solidity
/// @custom:oz-upgrades-unsafe-allow state-variable-immutable
address private immutable __self = address(this)
```

### UPGRADE_INTERFACE_VERSION (inherited from UUPSUpgradeable)

```solidity
///  @dev The version of the upgrade interface of the contract. If this getter is missing, both `upgradeTo(address)`
///  and `upgradeToAndCall(address,bytes)` are present, and `upgradeTo` must be used if no function should be called,
///  while `upgradeToAndCall` will invoke the `receive` function if the second argument is the empty byte string.
///  If the getter returns `"5.0.0"`, only `upgradeToAndCall(address,bytes)` is present, and the second argument must
///  be the empty byte string if no function should be called, making it impossible to invoke the `receive` function
///  during an upgrade.
string public constant UPGRADE_INTERFACE_VERSION = "5.0.0"
```

### DEFAULT_ADMIN_ROLE (inherited from AccessControlUpgradeable)

```solidity
bytes32 public constant DEFAULT_ADMIN_ROLE = 0x00
```

### AccessControlStorageLocation (inherited from AccessControlUpgradeable)

```solidity
bytes32 private constant AccessControlStorageLocation = 0x02dd7bc7dec4dceedda775e58dd541e08a116c6c53815c0bd028192f7b626800
```

### AccessControlEnumerableStorageLocation (inherited from AccessControlEnumerableUpgradeable)

```solidity
bytes32 private constant AccessControlEnumerableStorageLocation = 0xc1f6fe24621ce81ec5827caf0253cadb74709b061630e6b55e82371705932000
```

### PausableStorageLocation (inherited from PausableUpgradeable)

```solidity
bytes32 private constant PausableStorageLocation = 0xcd5ed15c6e187e77e9aee88184c21f4f2182ab5827cb3b7e07fbedcd63f03300
```

## Structs

### InitializableStorage (inherited from Initializable)

```solidity
///  @dev Storage of the initializable contract.
///  It's implemented on a custom ERC-7201 namespace to reduce the risk of storage collisions
///  when using with upgradeable contracts.
///  @custom:storage-location erc7201:openzeppelin.storage.Initializable
struct InitializableStorage {
    uint64 _initialized;
    bool _initializing;
}
```

### RoleData (inherited from AccessControlUpgradeable)

```solidity
struct RoleData {
    mapping(address => bool) hasRole;
    bytes32 adminRole;
}
```

### AccessControlStorage (inherited from AccessControlUpgradeable)

```solidity
/// @custom:storage-location erc7201:openzeppelin.storage.AccessControl
struct AccessControlStorage {
    mapping(bytes32 => RoleData) _roles;
}
```

### AccessControlEnumerableStorage (inherited from AccessControlEnumerableUpgradeable)

```solidity
/// @custom:storage-location erc7201:openzeppelin.storage.AccessControlEnumerable
struct AccessControlEnumerableStorage {
    mapping(bytes32 => EnumerableSet.AddressSet) _roleMembers;
}
```

### PausableStorage (inherited from PausableUpgradeable)

```solidity
/// @custom:storage-location erc7201:openzeppelin.storage.Pausable
struct PausableStorage {
    bool _paused;
}
```

## Errors

### InvalidInitialization (inherited from Initializable)

```solidity
///  @dev The contract is already initialized.
error InvalidInitialization();
```

### NotInitializing (inherited from Initializable)

```solidity
///  @dev The contract is not initializing.
error NotInitializing();
```

### UUPSUnauthorizedCallContext (inherited from UUPSUpgradeable)

```solidity
///  @dev The call is from an unauthorized context.
error UUPSUnauthorizedCallContext();
```

### UUPSUnsupportedProxiableUUID (inherited from UUPSUpgradeable)

```solidity
///  @dev The storage `slot` is unsupported as a UUID.
error UUPSUnsupportedProxiableUUID(bytes32 slot);
```

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

### EnforcedPause (inherited from PausableUpgradeable)

```solidity
///  @dev The operation failed because the contract is paused.
error EnforcedPause();
```

### ExpectedPause (inherited from PausableUpgradeable)

```solidity
///  @dev The operation failed because the contract is not paused.
error ExpectedPause();
```

### NullAddress

```solidity
error NullAddress();
```

## Events

### Initialized (inherited from Initializable)

```solidity
///  @dev Triggered when the contract has been initialized or reinitialized.
event Initialized(uint64 version);
```

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

### Paused (inherited from PausableUpgradeable)

```solidity
///  @dev Emitted when the pause is triggered by `account`.
event Paused(address account);
```

### Unpaused (inherited from PausableUpgradeable)

```solidity
///  @dev Emitted when the pause is lifted by `account`.
event Unpaused(address account);
```

## Public/External Functions

### constructor()

- **Signature**: `constructor()`
- **Visibility**: public
- **Source Range**: 1161:53:63
- **Details**: [function_constructor.md](./function_constructor.md)

**Signature:**
```solidity
/// @custom:oz-upgrades-unsafe-allow constructor
constructor();
```

### initialize(address)

- **Signature**: `initialize(address)`
- **Visibility**: public
- **Source Range**: 1349:441:63
- **Details**: [function_initialize_address.md](./function_initialize_address.md)

**Signature:**
```solidity
/// @notice Initializes the Auth contract with an admin address
///  @dev Grants all necessary roles to the admin address
function initialize(address admin_) public initializer();
```

### pause()

- **Signature**: `pause()`
- **Visibility**: external
- **Source Range**: 2129:73:63
- **Details**: [function_pause.md](./function_pause.md)

**Signature:**
```solidity
/// @notice Pauses the contract
///  @dev Only addresses with PAUSER_ROLE can pause the contract
function pause() external onlyRole(PAUSER_ROLE);
```

### unpause()

- **Signature**: `unpause()`
- **Visibility**: external
- **Source Range**: 2316:77:63
- **Details**: [function_unpause.md](./function_unpause.md)

**Signature:**
```solidity
/// @notice Unpauses the contract
///  @dev Only addresses with PAUSER_ROLE can unpause the contract
function unpause() external onlyRole(PAUSER_ROLE);
```

### proxiableUUID() (inherited from UUPSUpgradeable)

- **Signature**: `proxiableUUID()`
- **Visibility**: external
- **Source Range**: 3708:134:14
- **Details**: [function_proxiableUUID.md](./function_proxiableUUID.md)

**Signature:**
```solidity
///  @dev Implementation of the ERC-1822 {proxiableUUID} function. This returns the storage slot used by the
///  implementation. It is used to validate the implementation's compatibility when performing an upgrade.
///  IMPORTANT: A proxy pointing at a proxiable contract should not be considered proxiable itself, because this risks
///  bricking a proxy that upgrades to it, by delegating to itself until out of gas. Thus it is critical that this
///  function revert if invoked through a proxy. This is guaranteed by the `notDelegated` modifier.
function proxiableUUID() virtual external view notDelegated() returns (bytes32);
```

### upgradeToAndCall(address,bytes) (inherited from UUPSUpgradeable)

- **Signature**: `upgradeToAndCall(address,bytes)`
- **Visibility**: public
- **Source Range**: 4161:214:14
- **Details**: [function_upgradeToAndCall_address_bytes.md](./function_upgradeToAndCall_address_bytes.md)

**Signature:**
```solidity
///  @dev Upgrade the implementation of the proxy to `newImplementation`, and subsequently execute the function call
///  encoded in `data`.
///  Calls {_authorizeUpgrade}.
///  Emits an {Upgraded} event.
///  @custom:oz-upgrades-unsafe-allow-reachable delegatecall
function upgradeToAndCall(address newImplementation, bytes memory data) virtual public payable onlyProxy();
```

### supportsInterface(bytes4) (inherited from ERC165Upgradeable)

- **Signature**: `supportsInterface(bytes4)`
- **Visibility**: public
- **Source Range**: 1035:146:24
- **Details**: [function_supportsInterface_bytes4.md](./function_supportsInterface_bytes4.md)

**Signature:**
```solidity
///  @dev See {IERC165-supportsInterface}.
function supportsInterface(bytes4 interfaceId) virtual public view returns (bool);
```

### hasRole(bytes32,address) (inherited from AccessControlUpgradeable)

- **Signature**: `hasRole(bytes32,address)`
- **Visibility**: public
- **Source Range**: 3732:207:11
- **Details**: [function_hasRole_bytes32_address.md](./function_hasRole_bytes32_address.md)

**Signature:**
```solidity
///  @dev Returns `true` if `account` has been granted `role`.
function hasRole(bytes32 role, address account) virtual public view returns (bool);
```

### getRoleAdmin(bytes32) (inherited from AccessControlUpgradeable)

- **Signature**: `getRoleAdmin(bytes32)`
- **Visibility**: public
- **Source Range**: 4759:191:11
- **Details**: [function_getRoleAdmin_bytes32.md](./function_getRoleAdmin_bytes32.md)

**Signature:**
```solidity
///  @dev Returns the admin role that controls `role`. See {grantRole} and
///  {revokeRole}.
///  To change a role's admin, use {_setRoleAdmin}.
function getRoleAdmin(bytes32 role) virtual public view returns (bytes32);
```

### grantRole(bytes32,address) (inherited from AccessControlUpgradeable)

- **Signature**: `grantRole(bytes32,address)`
- **Visibility**: public
- **Source Range**: 5246:136:11
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

### revokeRole(bytes32,address) (inherited from AccessControlUpgradeable)

- **Signature**: `revokeRole(bytes32,address)`
- **Visibility**: public
- **Source Range**: 5662:138:11
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

### renounceRole(bytes32,address) (inherited from AccessControlUpgradeable)

- **Signature**: `renounceRole(bytes32,address)`
- **Visibility**: public
- **Source Range**: 6348:245:11
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

### getRoleMember(bytes32,uint256) (inherited from AccessControlEnumerableUpgradeable)

- **Signature**: `getRoleMember(bytes32,uint256)`
- **Visibility**: public
- **Source Range**: 2492:233:12
- **Details**: [function_getRoleMember_bytes32_uint256.md](./function_getRoleMember_bytes32_uint256.md)

**Signature:**
```solidity
///  @dev Returns one of the accounts that have `role`. `index` must be a
///  value between 0 and {getRoleMemberCount}, non-inclusive.
///  Role bearers are not sorted in any particular way, and their ordering may
///  change at any point.
///  WARNING: When using {getRoleMember} and {getRoleMemberCount}, make sure
///  you perform all queries on the same block. See the following
///  https://forum.openzeppelin.com/t/iterating-over-elements-on-enumerableset-in-openzeppelin-contracts/2296[forum post]
///  for more information.
function getRoleMember(bytes32 role, uint256 index) virtual public view returns (address);
```

### getRoleMemberCount(bytes32) (inherited from AccessControlEnumerableUpgradeable)

- **Signature**: `getRoleMemberCount(bytes32)`
- **Visibility**: public
- **Source Range**: 2893:222:12
- **Details**: [function_getRoleMemberCount_bytes32.md](./function_getRoleMemberCount_bytes32.md)

**Signature:**
```solidity
///  @dev Returns the number of accounts that have `role`. Can be used
///  together with {getRoleMember} to enumerate all bearers of a role.
function getRoleMemberCount(bytes32 role) virtual public view returns (uint256);
```

### getRoleMembers(bytes32) (inherited from AccessControlEnumerableUpgradeable)

- **Signature**: `getRoleMembers(bytes32)`
- **Visibility**: public
- **Source Range**: 3658:227:12
- **Details**: [function_getRoleMembers_bytes32.md](./function_getRoleMembers_bytes32.md)

**Signature:**
```solidity
///  @dev Return all accounts that have `role`
///  WARNING: This operation will copy the entire storage to memory, which can be quite expensive. This is designed
///  to mostly be used by view accessors that are queried without any gas fees. Developers should keep in mind that
///  this function has an unbounded cost, and using it as part of a state-changing function may render the function
///  uncallable if the set grows to a point where copying to memory consumes too much gas to fit in a block.
function getRoleMembers(bytes32 role) virtual public view returns (address[] memory);
```

### paused() (inherited from PausableUpgradeable)

- **Signature**: `paused()`
- **Visibility**: public
- **Source Range**: 2496:145:21
- **Details**: [function_paused.md](./function_paused.md)

**Signature:**
```solidity
///  @dev Returns true if the contract is paused, and false otherwise.
function paused() virtual public view returns (bool);
```

### multicall(bytes[]) (inherited from MulticallUpgradeable)

- **Signature**: `multicall(bytes[])`
- **Visibility**: external
- **Source Range**: 1518:484:19
- **Details**: [function_multicall_bytes[].md](./function_multicall_bytes[].md)

**Signature:**
```solidity
///  @dev Receives and executes a batch of function calls on this contract.
///  @custom:oz-upgrades-unsafe-allow-reachable delegatecall
function multicall(bytes[] calldata data) virtual external returns (bytes[] memory results);
```
