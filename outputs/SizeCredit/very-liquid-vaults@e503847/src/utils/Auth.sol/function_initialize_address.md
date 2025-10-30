# Function: initialize(address)

**Contract**: [src/utils/Auth.sol/contract_Auth.md]

## Metadata

- **Contract**: Auth
- **Signature**: `initialize(address)`
- **Visibility**: public
- **Source Range**: 1349:441:63

## Implementation

```solidity
/// @notice Initializes the Auth contract with an admin address
///  @dev Grants all necessary roles to the admin address
function initialize(address admin_) public initializer() {
    if (admin_ == address(0)) {
        revert NullAddress();
    }
    __AccessControl_init();
    __AccessControlEnumerable_init();
    __Pausable_init();
    __Multicall_init();
    __UUPSUpgradeable_init();
    _grantRole(DEFAULT_ADMIN_ROLE, admin_);
    _grantRole(PAUSER_ROLE, admin_);
    _grantRole(STRATEGIST_ROLE, admin_);
}
```

## Related Implementations

### __AccessControl_init()

- **Kind**: internal
- **Source**: 3231:65:11
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/access/AccessControlUpgradeable.sol:AccessControlUpgradeable:__AccessControl_init()`

```solidity
function __AccessControl_init() internal onlyInitializing() {}
```

### onlyInitializing()

- **Kind**: modifier
- **Source**: 6891:76:13
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/proxy/utils/Initializable.sol:Initializable:onlyInitializing()`

```solidity
///  @dev Modifier to protect an initialization function so that it can only be invoked by functions with the
///  {initializer} and {reinitializer} modifiers, directly or indirectly.
modifier onlyInitializing() {
    _checkInitializing();
    _;
}
```

### _checkInitializing()

- **Kind**: internal
- **Source**: 7082:141:13
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/proxy/utils/Initializable.sol:Initializable:_checkInitializing()`

```solidity
///  @dev Reverts if the contract is not in an initializing state. See {onlyInitializing}.
function _checkInitializing() virtual internal view {
    if (!_isInitializing()) {
        revert NotInitializing();
    }
}
```

### _isInitializing()

- **Kind**: internal
- **Source**: 8485:120:13
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/proxy/utils/Initializable.sol:Initializable:_isInitializing()`

```solidity
///  @dev Returns `true` if the contract is currently initializing. See {onlyInitializing}.
function _isInitializing() internal view returns (bool) {
    return _getInitializableStorage()._initializing;
}
```

### _getInitializableStorage()

- **Kind**: internal
- **Source**: 9071:205:13
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/proxy/utils/Initializable.sol:Initializable:_getInitializableStorage()`

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
- **Source**: 8819:122:13
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/proxy/utils/Initializable.sol:Initializable:_initializableStorageSlot()`

```solidity
///  @dev Pointer to storage slot. Allows integrators to override it with a custom storage location.
///  NOTE: Consider following the ERC-7201 formula to derive storage locations.
function _initializableStorageSlot() virtual internal pure returns (bytes32) {
    return INITIALIZABLE_STORAGE;
}
```

### __AccessControlEnumerable_init()

- **Kind**: internal
- **Source**: 1463:75:12
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/access/extensions/AccessControlEnumerableUpgradeable.sol:AccessControlEnumerableUpgradeable:__AccessControlEnumerable_init()`

```solidity
function __AccessControlEnumerable_init() internal onlyInitializing() {}
```

### __Pausable_init()

- **Kind**: internal
- **Source**: 2266:60:21
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/utils/PausableUpgradeable.sol:PausableUpgradeable:__Pausable_init()`

```solidity
function __Pausable_init() internal onlyInitializing() {}
```

### __Multicall_init()

- **Kind**: internal
- **Source**: 1218:61:19
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/utils/MulticallUpgradeable.sol:MulticallUpgradeable:__Multicall_init()`

```solidity
function __Multicall_init() internal onlyInitializing() {}
```

### __UUPSUpgradeable_init()

- **Kind**: internal
- **Source**: 2970:67:14
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/proxy/utils/UUPSUpgradeable.sol:UUPSUpgradeable:__UUPSUpgradeable_init()`

```solidity
function __UUPSUpgradeable_init() internal onlyInitializing() {}
```

### _grantRole(bytes32,address)

- **Kind**: internal
- **Source**: 3987:348:12
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/access/extensions/AccessControlEnumerableUpgradeable.sol:AccessControlEnumerableUpgradeable:_grantRole(bytes32,address)`

```solidity
///  @dev Overload {AccessControl-_grantRole} to track enumerable memberships
function _grantRole(bytes32 role, address account) virtual override internal returns (bool) {
    AccessControlEnumerableStorage storage $ = _getAccessControlEnumerableStorage();
    bool granted = super._grantRole(role, account);
    if (granted) {
        $._roleMembers[role].add(account);
    }
    return granted;
}
```

### _getAccessControlEnumerableStorage()

- **Kind**: internal
- **Source**: 1250:207:12
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/access/extensions/AccessControlEnumerableUpgradeable.sol:AccessControlEnumerableUpgradeable:_getAccessControlEnumerableStorage()`

```solidity
function _getAccessControlEnumerableStorage() private pure returns (AccessControlEnumerableStorage storage $) {
    assembly {
        $.slot := AccessControlEnumerableStorageLocation
    }
}
```

### _grantRole(bytes32,address)

- **Kind**: internal
- **Source**: 7270:387:11
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
- **Source**: 2787:177:11
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
- **Source**: 3732:207:11
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
- **Source**: 887:96:18
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/utils/ContextUpgradeable.sol:ContextUpgradeable:_msgSender()`

```solidity
function _msgSender() virtual internal view returns (address) {
    return msg.sender;
}
```

### add(struct EnumerableSet.AddressSet,address)

- **Kind**: internal
- **Source**: 9332:150:55
- **Link**: `lib/openzeppelin-contracts/contracts/utils/structs/EnumerableSet.sol:EnumerableSet:add(struct EnumerableSet.AddressSet,address)`

```solidity
///  @dev Add a value to a set. O(1).
///  Returns true if the value was added to the set, that is if it was not
///  already present.
function add(AddressSet storage set, address value) internal returns (bool) {
    return _add(set._inner, bytes32(uint256(uint160(value))));
}
```

### _add(struct EnumerableSet.Set,bytes32)

- **Kind**: internal
- **Source**: 2336:406:55
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
- **Source**: 4910:129:55
- **Link**: `lib/openzeppelin-contracts/contracts/utils/structs/EnumerableSet.sol:EnumerableSet:_contains(struct EnumerableSet.Set,bytes32)`

```solidity
///  @dev Returns true if the value is in the set. O(1).
function _contains(Set storage set, bytes32 value) private view returns (bool) {
    return set._positions[value] != 0;
}
```

### initializer()

- **Kind**: modifier
- **Source**: 4069:1102:13
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/proxy/utils/Initializable.sol:Initializable:initializer()`

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

## State Variable Reads

- **INITIALIZABLE_STORAGE** (`bytes32`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: Auth.initialize(address) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
  ├─ [1] ⚙️ FUNCTION: AccessControlUpgradeable.__AccessControl_init() (NodeID: 1)
  │   💬 Args: [no args]
  │   👁️  Def: internal
  │ └─ [2] 🔒 MODIFIER: Initializable.onlyInitializing() (NodeID: 2)
  │     💬 Args: [no args]
  │   └─ [3] ⚙️ FUNCTION: Initializable._checkInitializing() (NodeID: 3)
  │       💬 Args: [no args]
  │       👁️  Def: internal
  │     └─ [4] ⚙️ FUNCTION: Initializable._isInitializing() (NodeID: 4)
  │         💬 Args: [no args]
  │         👁️  Def: internal
  │       └─ [5] ⚙️ FUNCTION: Initializable._getInitializableStorage() (NodeID: 5)
  │           💬 Args: [no args]
  │           👁️  Def: private
  │         └─ [6] ⚙️ FUNCTION: Initializable._initializableStorageSlot() (NodeID: 6)
  │             💬 Args: [no args]
  │             👁️  Def: internal
  ├─ [1] ⚙️ FUNCTION: AccessControlEnumerableUpgradeable.__AccessControlEnumerable_init() (NodeID: 7)
  │   💬 Args: [no args]
  │   👁️  Def: internal
  │ └─ [2] 🔒 MODIFIER: Initializable.onlyInitializing() (NodeID: 8)
  │     💬 Args: [no args]
  │   └─ [3] ⚙️ FUNCTION: Initializable._checkInitializing() (NodeID: 9)
  │       💬 Args: [no args]
  │       👁️  Def: internal
  │     └─ [4] ⚙️ FUNCTION: Initializable._isInitializing() (NodeID: 10)
  │         💬 Args: [no args]
  │         👁️  Def: internal
  │       └─ [5] ⚙️ FUNCTION: Initializable._getInitializableStorage() (NodeID: 11)
  │           💬 Args: [no args]
  │           👁️  Def: private
  │         └─ [6] ⚙️ FUNCTION: Initializable._initializableStorageSlot() (NodeID: 12)
  │             💬 Args: [no args]
  │             👁️  Def: internal
  ├─ [1] ⚙️ FUNCTION: PausableUpgradeable.__Pausable_init() (NodeID: 13)
  │   💬 Args: [no args]
  │   👁️  Def: internal
  │ └─ [2] 🔒 MODIFIER: Initializable.onlyInitializing() (NodeID: 14)
  │     💬 Args: [no args]
  │   └─ [3] ⚙️ FUNCTION: Initializable._checkInitializing() (NodeID: 15)
  │       💬 Args: [no args]
  │       👁️  Def: internal
  │     └─ [4] ⚙️ FUNCTION: Initializable._isInitializing() (NodeID: 16)
  │         💬 Args: [no args]
  │         👁️  Def: internal
  │       └─ [5] ⚙️ FUNCTION: Initializable._getInitializableStorage() (NodeID: 17)
  │           💬 Args: [no args]
  │           👁️  Def: private
  │         └─ [6] ⚙️ FUNCTION: Initializable._initializableStorageSlot() (NodeID: 18)
  │             💬 Args: [no args]
  │             👁️  Def: internal
  ├─ [1] ⚙️ FUNCTION: MulticallUpgradeable.__Multicall_init() (NodeID: 19)
  │   💬 Args: [no args]
  │   👁️  Def: internal
  │ └─ [2] 🔒 MODIFIER: Initializable.onlyInitializing() (NodeID: 20)
  │     💬 Args: [no args]
  │   └─ [3] ⚙️ FUNCTION: Initializable._checkInitializing() (NodeID: 21)
  │       💬 Args: [no args]
  │       👁️  Def: internal
  │     └─ [4] ⚙️ FUNCTION: Initializable._isInitializing() (NodeID: 22)
  │         💬 Args: [no args]
  │         👁️  Def: internal
  │       └─ [5] ⚙️ FUNCTION: Initializable._getInitializableStorage() (NodeID: 23)
  │           💬 Args: [no args]
  │           👁️  Def: private
  │         └─ [6] ⚙️ FUNCTION: Initializable._initializableStorageSlot() (NodeID: 24)
  │             💬 Args: [no args]
  │             👁️  Def: internal
  ├─ [1] ⚙️ FUNCTION: UUPSUpgradeable.__UUPSUpgradeable_init() (NodeID: 25)
  │   💬 Args: [no args]
  │   👁️  Def: internal
  │ └─ [2] 🔒 MODIFIER: Initializable.onlyInitializing() (NodeID: 26)
  │     💬 Args: [no args]
  │   └─ [3] ⚙️ FUNCTION: Initializable._checkInitializing() (NodeID: 27)
  │       💬 Args: [no args]
  │       👁️  Def: internal
  │     └─ [4] ⚙️ FUNCTION: Initializable._isInitializing() (NodeID: 28)
  │         💬 Args: [no args]
  │         👁️  Def: internal
  │       └─ [5] ⚙️ FUNCTION: Initializable._getInitializableStorage() (NodeID: 29)
  │           💬 Args: [no args]
  │           👁️  Def: private
  │         └─ [6] ⚙️ FUNCTION: Initializable._initializableStorageSlot() (NodeID: 30)
  │             💬 Args: [no args]
  │             👁️  Def: internal
  ├─ [1] ⚙️ FUNCTION: AccessControlEnumerableUpgradeable._grantRole(bytes32,address) (NodeID: 31)
  │   💬 Args: [DEFAULT_ADMIN_ROLE, admin_]
  │   👁️  Def: internal
  │ ├─ [2] ⚙️ FUNCTION: AccessControlEnumerableUpgradeable._getAccessControlEnumerableStorage() (NodeID: 32)
  │ │   💬 Args: [no args]
  │ │   👁️  Def: private
  │ ├─ [2] ⚙️ FUNCTION: AccessControlUpgradeable._grantRole(bytes32,address) (NodeID: 33)
  │ │   💬 Args: [role, account]
  │ │   👁️  Def: internal
  │ │ ├─ [3] ⚙️ FUNCTION: AccessControlUpgradeable._getAccessControlStorage() (NodeID: 34)
  │ │ │   💬 Args: [no args]
  │ │ │   👁️  Def: private
  │ │ ├─ [3] ⚙️ FUNCTION: AccessControlUpgradeable.hasRole(bytes32,address) (NodeID: 35)
  │ │ │   💬 Args: [role, account]
  │ │ │   👁️  Def: public
  │ │ │ └─ [4] ⚙️ FUNCTION: AccessControlUpgradeable._getAccessControlStorage() (NodeID: 36)
  │ │ │     💬 Args: [no args]
  │ │ │     👁️  Def: private
  │ │ └─ [3] ⚙️ FUNCTION: ContextUpgradeable._msgSender() (NodeID: 37)
  │ │     💬 Args: [no args]
  │ │     👁️  Def: internal
  │ └─ [2] ⚙️ FUNCTION: EnumerableSet.add(struct EnumerableSet.AddressSet,address) (NodeID: 38)
  │     💬 Args: [$._roleMembers[role], account]
  │     👁️  Def: internal
  │   └─ [3] ⚙️ FUNCTION: EnumerableSet._add(struct EnumerableSet.Set,bytes32) (NodeID: 39)
  │       💬 Args: [set._inner, bytes32(uint256(uint160(value)))]
  │       👁️  Def: private
  │     └─ [4] ⚙️ FUNCTION: EnumerableSet._contains(struct EnumerableSet.Set,bytes32) (NodeID: 40)
  │         💬 Args: [set, value]
  │         👁️  Def: private
  ├─ [1] ⚙️ FUNCTION: AccessControlEnumerableUpgradeable._grantRole(bytes32,address) (NodeID: 41)
  │   💬 Args: [PAUSER_ROLE, admin_]
  │   👁️  Def: internal
  │ ├─ [2] ⚙️ FUNCTION: AccessControlEnumerableUpgradeable._getAccessControlEnumerableStorage() (NodeID: 42)
  │ │   💬 Args: [no args]
  │ │   👁️  Def: private
  │ ├─ [2] ⚙️ FUNCTION: AccessControlUpgradeable._grantRole(bytes32,address) (NodeID: 43)
  │ │   💬 Args: [role, account]
  │ │   👁️  Def: internal
  │ │ ├─ [3] ⚙️ FUNCTION: AccessControlUpgradeable._getAccessControlStorage() (NodeID: 44)
  │ │ │   💬 Args: [no args]
  │ │ │   👁️  Def: private
  │ │ ├─ [3] ⚙️ FUNCTION: AccessControlUpgradeable.hasRole(bytes32,address) (NodeID: 45)
  │ │ │   💬 Args: [role, account]
  │ │ │   👁️  Def: public
  │ │ │ └─ [4] ⚙️ FUNCTION: AccessControlUpgradeable._getAccessControlStorage() (NodeID: 46)
  │ │ │     💬 Args: [no args]
  │ │ │     👁️  Def: private
  │ │ └─ [3] ⚙️ FUNCTION: ContextUpgradeable._msgSender() (NodeID: 47)
  │ │     💬 Args: [no args]
  │ │     👁️  Def: internal
  │ └─ [2] ⚙️ FUNCTION: EnumerableSet.add(struct EnumerableSet.AddressSet,address) (NodeID: 48)
  │     💬 Args: [$._roleMembers[role], account]
  │     👁️  Def: internal
  │   └─ [3] ⚙️ FUNCTION: EnumerableSet._add(struct EnumerableSet.Set,bytes32) (NodeID: 49)
  │       💬 Args: [set._inner, bytes32(uint256(uint160(value)))]
  │       👁️  Def: private
  │     └─ [4] ⚙️ FUNCTION: EnumerableSet._contains(struct EnumerableSet.Set,bytes32) (NodeID: 50)
  │         💬 Args: [set, value]
  │         👁️  Def: private
  ├─ [1] ⚙️ FUNCTION: AccessControlEnumerableUpgradeable._grantRole(bytes32,address) (NodeID: 51)
  │   💬 Args: [STRATEGIST_ROLE, admin_]
  │   👁️  Def: internal
  │ ├─ [2] ⚙️ FUNCTION: AccessControlEnumerableUpgradeable._getAccessControlEnumerableStorage() (NodeID: 52)
  │ │   💬 Args: [no args]
  │ │   👁️  Def: private
  │ ├─ [2] ⚙️ FUNCTION: AccessControlUpgradeable._grantRole(bytes32,address) (NodeID: 53)
  │ │   💬 Args: [role, account]
  │ │   👁️  Def: internal
  │ │ ├─ [3] ⚙️ FUNCTION: AccessControlUpgradeable._getAccessControlStorage() (NodeID: 54)
  │ │ │   💬 Args: [no args]
  │ │ │   👁️  Def: private
  │ │ ├─ [3] ⚙️ FUNCTION: AccessControlUpgradeable.hasRole(bytes32,address) (NodeID: 55)
  │ │ │   💬 Args: [role, account]
  │ │ │   👁️  Def: public
  │ │ │ └─ [4] ⚙️ FUNCTION: AccessControlUpgradeable._getAccessControlStorage() (NodeID: 56)
  │ │ │     💬 Args: [no args]
  │ │ │     👁️  Def: private
  │ │ └─ [3] ⚙️ FUNCTION: ContextUpgradeable._msgSender() (NodeID: 57)
  │ │     💬 Args: [no args]
  │ │     👁️  Def: internal
  │ └─ [2] ⚙️ FUNCTION: EnumerableSet.add(struct EnumerableSet.AddressSet,address) (NodeID: 58)
  │     💬 Args: [$._roleMembers[role], account]
  │     👁️  Def: internal
  │   └─ [3] ⚙️ FUNCTION: EnumerableSet._add(struct EnumerableSet.Set,bytes32) (NodeID: 59)
  │       💬 Args: [set._inner, bytes32(uint256(uint160(value)))]
  │       👁️  Def: private
  │     └─ [4] ⚙️ FUNCTION: EnumerableSet._contains(struct EnumerableSet.Set,bytes32) (NodeID: 60)
  │         💬 Args: [set, value]
  │         👁️  Def: private
  └─ [1] 🔒 MODIFIER: Initializable.initializer() (NodeID: 61)
      💬 Args: [no args]
    └─ [2] ⚙️ FUNCTION: Initializable._getInitializableStorage() (NodeID: 62)
        💬 Args: [no args]
        👁️  Def: private
      └─ [3] ⚙️ FUNCTION: Initializable._initializableStorageSlot() (NodeID: 63)
          💬 Args: [no args]
          👁️  Def: internal
```

## Documentation

### Function Documentation

@notice Initializes the Auth contract with an admin address
 @dev Grants all necessary roles to the admin address
