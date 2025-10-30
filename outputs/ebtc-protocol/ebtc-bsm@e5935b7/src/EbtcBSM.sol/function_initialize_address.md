# Function: initialize(address)

**Contract**: [src/EbtcBSM.sol/contract_EbtcBSM.md]

## Metadata

- **Contract**: EbtcBSM
- **Signature**: `initialize(address)`
- **Visibility**: external
- **Source Range**: 4401:140:40

## Implementation

```solidity
/// @notice This function will be invoked only once within the same transaction as the deployment of
///  this contract, thereby preventing any other user from executing this function.
///  @param _escrow Address of the escrow contract
function initialize(address _escrow) external initializer() {
    require(_escrow != address(0));
    escrow = IEscrow(_escrow);
}
```

## Related Implementations

### initializer()

- **Kind**: modifier
- **Source**: 4069:1104:6
- **Link**: `lib/openzeppelin-contracts/contracts/proxy/utils/Initializable.sol:Initializable:initializer()`

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
- **Source**: 8737:170:6
- **Link**: `lib/openzeppelin-contracts/contracts/proxy/utils/Initializable.sol:Initializable:_getInitializableStorage()`

```solidity
///  @dev Returns a pointer to the storage namespace.
function _getInitializableStorage() private pure returns (InitializableStorage storage $) {
    assembly {
        $.slot := INITIALIZABLE_STORAGE
    }
}
```

## State Variable Writes

- **escrow** (`contract IEscrow`) [src/Dependencies/IEscrow.sol/interface_IEscrow.md]

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: EbtcBSM.initialize(address) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: external
  └─ [1] 🔒 MODIFIER: Initializable.initializer() (NodeID: 1)
      💬 Args: [no args]
    └─ [2] ⚙️ FUNCTION: Initializable._getInitializableStorage() (NodeID: 2)
        💬 Args: [no args]
        👁️  Def: private
```

## Documentation

### Function Documentation

@notice This function will be invoked only once within the same transaction as the deployment of
 this contract, thereby preventing any other user from executing this function.
 @param _escrow Address of the escrow contract
