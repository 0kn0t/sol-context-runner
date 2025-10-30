# Function: initialize(address)

**Contract**: [src/TokenHolder.sol/contract_TokenHolder.md]

## Metadata

- **Contract**: TokenHolder
- **Signature**: `initialize(address)`
- **Visibility**: public
- **Source Range**: 1911:166:13

## Implementation

```solidity
function initialize(address _tokenAddress) public initializer() {
    tokenHolded = IERC20(_tokenAddress);
    _grantRole(DEFAULT_ADMIN_ROLE, msg.sender);
}
```

## Related Implementations

### _grantRole(bytes32,address)

- **Kind**: internal
- **Source**: 7339:387:0
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/access/AccessControlUpgradeable.sol:AccessControlUpgradeable:_grantRole(bytes32,address)`

```solidity
///  @dev Attempts to grant `role` to `account` and returns a boolean indicating if `role` was granted.
///  Internal function without access restriction.
///  May emit a {RoleGranted} event.
function _grantRole(bytes32 role, address account) virtual internal returns (bool) {
    AccessControlStorage storage $ = _getAccessControlStorage();
    if (!hasRole(role, account)) {
        $._roles[role].hasRole[account] = true;
        emit RoleGranted(role, account, _msgSender());
        return true;
    } else {
        return false;
    }
}
```

### _getAccessControlStorage()

- **Kind**: internal
- **Source**: 2889:177:0
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/access/AccessControlUpgradeable.sol:AccessControlUpgradeable:_getAccessControlStorage()`

```solidity
function _getAccessControlStorage() private pure returns (AccessControlStorage storage $) {
    assembly {
        $.slot := AccessControlStorageLocation
    }
}
```

### hasRole(bytes32,address)

- **Kind**: internal
- **Source**: 3801:207:0
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/access/AccessControlUpgradeable.sol:AccessControlUpgradeable:hasRole(bytes32,address)`

```solidity
///  @dev Returns `true` if `account` has been granted `role`.
function hasRole(bytes32 role, address account) virtual public view returns (bool) {
    AccessControlStorage storage $ = _getAccessControlStorage();
    return $._roles[role].hasRole[account];
}
```

### _msgSender()

- **Kind**: internal
- **Source**: 908:96:2
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/utils/ContextUpgradeable.sol:ContextUpgradeable:_msgSender()`

```solidity
function _msgSender() virtual internal view returns (address) {
    return msg.sender;
}
```

### initializer()

- **Kind**: modifier
- **Source**: 4069:1102:7
- **Link**: `lib/openzeppelin-contracts-upgradeable/lib/openzeppelin-contracts/contracts/proxy/utils/Initializable.sol:Initializable:initializer()`

```solidity
///  @dev A modifier that defines a protected initializer function that can be invoked at most once. In its scope,
///  `onlyInitializing` functions can be used to initialize parent contracts.
///  Similar to `reinitializer(1)`, except that in the context of a constructor an `initializer` may be invoked any
///  number of times. This behavior in the constructor can be useful during testing and is not expected to be used in
///  production.
///  Emits an {Initialized} event.
modifier initializer() {
    InitializableStorage storage $ = _getInitializableStorage();
    bool isTopLevelCall = !$._initializing;
    uint64 initialized = $._initialized;
    bool initialSetup = (initialized == 0) && isTopLevelCall;
    bool construction = (initialized == 1) && (address(this).code.length == 0);
    if ((!initialSetup) && (!construction)) {
        revert InvalidInitialization();
    }
    $._initialized = 1;
    if (isTopLevelCall) {
        $._initializing = true;
    }
    _;
    if (isTopLevelCall) {
        $._initializing = false;
        emit Initialized(1);
    }
}
```

### _getInitializableStorage()

- **Kind**: internal
- **Source**: 9071:205:7
- **Link**: `lib/openzeppelin-contracts-upgradeable/lib/openzeppelin-contracts/contracts/proxy/utils/Initializable.sol:Initializable:_getInitializableStorage()`

```solidity
///  @dev Returns a pointer to the storage namespace.
function _getInitializableStorage() private pure returns (InitializableStorage storage $) {
    bytes32 slot = _initializableStorageSlot();
    assembly {
        $.slot := slot
    }
}
```

### _initializableStorageSlot()

- **Kind**: internal
- **Source**: 8819:122:7
- **Link**: `lib/openzeppelin-contracts-upgradeable/lib/openzeppelin-contracts/contracts/proxy/utils/Initializable.sol:Initializable:_initializableStorageSlot()`

```solidity
///  @dev Pointer to storage slot. Allows integrators to override it with a custom storage location.
///  NOTE: Consider following the ERC-7201 formula to derive storage locations.
function _initializableStorageSlot() virtual internal pure returns (bytes32) {
    return INITIALIZABLE_STORAGE;
}
```

## State Variable Reads

- **INITIALIZABLE_STORAGE** (`bytes32`)

## State Variable Writes

- **tokenHolded** (`contract IERC20`) [lib/openzeppelin-contracts-upgradeable/lib/openzeppelin-contracts/contracts/token/ERC20/IERC20.sol/interface_IERC20.md]

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: TokenHolder.initialize(address) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
  ├─ [1] ⚙️ FUNCTION: AccessControlUpgradeable._grantRole(bytes32,address) (NodeID: 1)
  │   💬 Args: [DEFAULT_ADMIN_ROLE, msg.sender]
  │   👁️  Def: internal
  │ ├─ [2] ⚙️ FUNCTION: AccessControlUpgradeable._getAccessControlStorage() (NodeID: 2)
  │ │   💬 Args: [no args]
  │ │   👁️  Def: private
  │ ├─ [2] ⚙️ FUNCTION: AccessControlUpgradeable.hasRole(bytes32,address) (NodeID: 3)
  │ │   💬 Args: [role, account]
  │ │   👁️  Def: public
  │ │ └─ [3] ⚙️ FUNCTION: AccessControlUpgradeable._getAccessControlStorage() (NodeID: 4)
  │ │     💬 Args: [no args]
  │ │     👁️  Def: private
  │ └─ [2] ⚙️ FUNCTION: ContextUpgradeable._msgSender() (NodeID: 5)
  │     💬 Args: [no args]
  │     👁️  Def: internal
  └─ [1] 🔒 MODIFIER: Initializable.initializer() (NodeID: 6)
      💬 Args: [no args]
    └─ [2] ⚙️ FUNCTION: Initializable._getInitializableStorage() (NodeID: 7)
        💬 Args: [no args]
        👁️  Def: private
      └─ [3] ⚙️ FUNCTION: Initializable._initializableStorageSlot() (NodeID: 8)
          💬 Args: [no args]
          👁️  Def: internal
```
