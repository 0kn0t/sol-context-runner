# Function: initialize(contract IERC20)

**Contract**: [src/BorrowerOperationsV6.sol/contract_BorrowerOperationsV6.md]

## Metadata

- **Contract**: BorrowerOperationsV6
- **Signature**: `initialize(contract IERC20)`
- **Visibility**: public
- **Source Range**: 1899:344:12

## Implementation

```solidity
function initialize(IERC20 _weth) public initializer() {
    weth = _weth;
    admin = msg.sender;
    openingFee = 50;
    profitFee = 800;
    maxApprovalAmount = 1000 ether;
    ReentrancyGuardUpgradeable.__ReentrancyGuard_init();
}
```

## Related Implementations

### __ReentrancyGuard_init()

- **Kind**: internal
- **Source**: 2778:111:3
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/utils/ReentrancyGuardUpgradeable.sol:ReentrancyGuardUpgradeable:__ReentrancyGuard_init()`

```solidity
function __ReentrancyGuard_init() internal onlyInitializing() {
    __ReentrancyGuard_init_unchained();
}
```

### __ReentrancyGuard_init_unchained()

- **Kind**: internal
- **Source**: 2895:153:3
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/utils/ReentrancyGuardUpgradeable.sol:ReentrancyGuardUpgradeable:__ReentrancyGuard_init_unchained()`

```solidity
function __ReentrancyGuard_init_unchained() internal onlyInitializing() {
    _reentrancyGuardStorageSlot().getUint256Slot().value = NOT_ENTERED;
}
```

### _reentrancyGuardStorageSlot()

- **Kind**: internal
- **Source**: 5072:127:3
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/utils/ReentrancyGuardUpgradeable.sol:ReentrancyGuardUpgradeable:_reentrancyGuardStorageSlot()`

```solidity
function _reentrancyGuardStorageSlot() virtual internal pure returns (bytes32) {
    return REENTRANCY_GUARD_STORAGE;
}
```

### getUint256Slot(bytes32)

- **Kind**: internal
- **Source**: 2679:163:10
- **Link**: `lib/openzeppelin-contracts-upgradeable/lib/openzeppelin-contracts/contracts/utils/StorageSlot.sol:StorageSlot:getUint256Slot(bytes32)`

```solidity
///  @dev Returns a `Uint256Slot` with member `value` located at `slot`.
function getUint256Slot(bytes32 slot) internal pure returns (Uint256Slot storage r) {
    assembly ("memory-safe") {
        r.slot := slot
    }
}
```

### onlyInitializing()

- **Kind**: modifier
- **Source**: 6891:76:7
- **Link**: `lib/openzeppelin-contracts-upgradeable/lib/openzeppelin-contracts/contracts/proxy/utils/Initializable.sol:Initializable:onlyInitializing()`

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
- **Source**: 7082:141:7
- **Link**: `lib/openzeppelin-contracts-upgradeable/lib/openzeppelin-contracts/contracts/proxy/utils/Initializable.sol:Initializable:_checkInitializing()`

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
- **Source**: 8485:120:7
- **Link**: `lib/openzeppelin-contracts-upgradeable/lib/openzeppelin-contracts/contracts/proxy/utils/Initializable.sol:Initializable:_isInitializing()`

```solidity
///  @dev Returns `true` if the contract is currently initializing. See {onlyInitializing}.
function _isInitializing() internal view returns (bool) {
    return _getInitializableStorage()._initializing;
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

## State Variable Reads

- **NOT_ENTERED** (`uint256`)
- **REENTRANCY_GUARD_STORAGE** (`bytes32`)
- **INITIALIZABLE_STORAGE** (`bytes32`)

## State Variable Writes

- **weth** (`contract IERC20`) [lib/openzeppelin-contracts-upgradeable/lib/openzeppelin-contracts/contracts/token/ERC20/IERC20.sol/interface_IERC20.md]
- **admin** (`address`)
- **openingFee** (`uint256`)
- **profitFee** (`uint256`)
- **maxApprovalAmount** (`uint256`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: BorrowerOperationsV6.initialize(contract IERC20) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
  ├─ [1] ⚙️ FUNCTION: ReentrancyGuardUpgradeable.__ReentrancyGuard_init() (NodeID: 1)
  │   💬 Args: [no args]
  │   👁️  Def: internal
  │ ├─ [2] ⚙️ FUNCTION: ReentrancyGuardUpgradeable.__ReentrancyGuard_init_unchained() (NodeID: 2)
  │ │   💬 Args: [no args]
  │ │   👁️  Def: internal
  │ │ ├─ [3] ⚙️ FUNCTION: ReentrancyGuardUpgradeable._reentrancyGuardStorageSlot() (NodeID: 3)
  │ │ │   💬 Args: [no args]
  │ │ │   👁️  Def: internal
  │ │ ├─ [3] ⚙️ FUNCTION: StorageSlot.getUint256Slot(bytes32) (NodeID: 4)
  │ │ │   💬 Args: [_reentrancyGuardStorageSlot()]
  │ │ │   👁️  Def: internal
  │ │ └─ [3] 🔒 MODIFIER: Initializable.onlyInitializing() (NodeID: 5)
  │ │     💬 Args: [no args]
  │ │   └─ [4] ⚙️ FUNCTION: Initializable._checkInitializing() (NodeID: 6)
  │ │       💬 Args: [no args]
  │ │       👁️  Def: internal
  │ │     └─ [5] ⚙️ FUNCTION: Initializable._isInitializing() (NodeID: 7)
  │ │         💬 Args: [no args]
  │ │         👁️  Def: internal
  │ │       └─ [6] ⚙️ FUNCTION: Initializable._getInitializableStorage() (NodeID: 8)
  │ │           💬 Args: [no args]
  │ │           👁️  Def: private
  │ │         └─ [7] ⚙️ FUNCTION: Initializable._initializableStorageSlot() (NodeID: 9)
  │ │             💬 Args: [no args]
  │ │             👁️  Def: internal
  │ └─ [2] 🔒 MODIFIER: Initializable.onlyInitializing() (NodeID: 10)
  │     💬 Args: [no args]
  │   └─ [3] ⚙️ FUNCTION: Initializable._checkInitializing() (NodeID: 11)
  │       💬 Args: [no args]
  │       👁️  Def: internal
  │     └─ [4] ⚙️ FUNCTION: Initializable._isInitializing() (NodeID: 12)
  │         💬 Args: [no args]
  │         👁️  Def: internal
  │       └─ [5] ⚙️ FUNCTION: Initializable._getInitializableStorage() (NodeID: 13)
  │           💬 Args: [no args]
  │           👁️  Def: private
  │         └─ [6] ⚙️ FUNCTION: Initializable._initializableStorageSlot() (NodeID: 14)
  │             💬 Args: [no args]
  │             👁️  Def: internal
  └─ [1] 🔒 MODIFIER: Initializable.initializer() (NodeID: 15)
      💬 Args: [no args]
    └─ [2] ⚙️ FUNCTION: Initializable._getInitializableStorage() (NodeID: 16)
        💬 Args: [no args]
        👁️  Def: private
      └─ [3] ⚙️ FUNCTION: Initializable._initializableStorageSlot() (NodeID: 17)
          💬 Args: [no args]
          👁️  Def: internal
```
