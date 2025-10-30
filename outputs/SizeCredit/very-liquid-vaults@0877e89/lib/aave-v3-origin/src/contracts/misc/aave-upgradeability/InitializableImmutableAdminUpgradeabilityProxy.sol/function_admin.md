# Function: admin()

**Contract**: [lib/aave-v3-origin/src/contracts/misc/aave-upgradeability/InitializableImmutableAdminUpgradeabilityProxy.sol/contract_InitializableImmutableAdminUpgradeabilityProxy.md]

## Metadata

- **Contract**: InitializableImmutableAdminUpgradeabilityProxy
- **Signature**: `admin()`
- **Visibility**: external
- **Source Range**: 1168:76:22
- **Inherited From**: BaseImmutableAdminUpgradeabilityProxy

## Implementation

```solidity
///  @notice Return the admin address
///  @return The address of the proxy admin.
function admin() external ifAdmin() returns (address) {
    return _admin;
}
```

## Related Implementations

### ifAdmin()

- **Kind**: modifier
- **Source**: 966:103:22
- **Link**: `lib/aave-v3-origin/src/contracts/misc/aave-upgradeability/BaseImmutableAdminUpgradeabilityProxy.sol:BaseImmutableAdminUpgradeabilityProxy:ifAdmin()`

```solidity
modifier ifAdmin() {
    if (msg.sender == _admin) {
        _;
    } else {
        _fallback();
    }
}
```

### _fallback()

- **Kind**: internal
- **Source**: 2355:90:9
- **Link**: `lib/aave-v3-origin/src/contracts/dependencies/openzeppelin/upgradeability/Proxy.sol:Proxy:_fallback()`

```solidity
///  @dev fallback implementation.
///  Extracted to enable manual triggering.
function _fallback() internal {
    _willFallback();
    _delegate(_implementation());
}
```

### _willFallback()

- **Kind**: internal
- **Source**: 904:153:23
- **Link**: `lib/aave-v3-origin/src/contracts/misc/aave-upgradeability/InitializableImmutableAdminUpgradeabilityProxy.sol:InitializableImmutableAdminUpgradeabilityProxy:_willFallback()`

```solidity
/// @inheritdoc BaseImmutableAdminUpgradeabilityProxy
function _willFallback() override(BaseImmutableAdminUpgradeabilityProxy, Proxy) internal {
    BaseImmutableAdminUpgradeabilityProxy._willFallback();
}
```

### _willFallback()

- **Kind**: internal
- **Source**: 2591:172:22
- **Link**: `lib/aave-v3-origin/src/contracts/misc/aave-upgradeability/BaseImmutableAdminUpgradeabilityProxy.sol:BaseImmutableAdminUpgradeabilityProxy:_willFallback()`

```solidity
///  @notice Only fall back when the sender is not the admin.
function _willFallback() virtual override internal {
    require(msg.sender != _admin, "Cannot call fallback function from the proxy admin");
    super._willFallback();
}
```

### _willFallback()

- **Kind**: internal
- **Source**: 2216:44:9
- **Link**: `lib/aave-v3-origin/src/contracts/dependencies/openzeppelin/upgradeability/Proxy.sol:Proxy:_willFallback()`

```solidity
///  @dev Function that is run as the first thing in the fallback function.
///  Can be redefined in derived contracts to add functionality.
///  Redefinitions must call super._willFallback().
function _willFallback() virtual internal {}
```

### _delegate(address)

- **Kind**: internal
- **Source**: 1205:802:9
- **Link**: `lib/aave-v3-origin/src/contracts/dependencies/openzeppelin/upgradeability/Proxy.sol:Proxy:_delegate(address)`

```solidity
///  @dev Delegates execution to an implementation contract.
///  This is a low level function that doesn't return to its internal call site.
///  It will return to the external caller whatever the implementation returns.
///  @param implementation Address to delegate.
function _delegate(address implementation) internal {
    assembly {
        calldatacopy(0, 0, calldatasize())
        let result := delegatecall(gas(), implementation, 0, calldatasize(), 0, 0)
        returndatacopy(0, 0, returndatasize())
        switch result
        case 0 {
            revert(0, returndatasize())
        }
        default {
            return(0, returndatasize())
        }
    }
}
```

### _implementation()

- **Kind**: internal
- **Source**: 1004:196:7
- **Link**: `lib/aave-v3-origin/src/contracts/dependencies/openzeppelin/upgradeability/BaseUpgradeabilityProxy.sol:BaseUpgradeabilityProxy:_implementation()`

```solidity
///  @dev Returns the current implementation.
///  @return impl Address of the current implementation
function _implementation() override internal view returns (address impl) {
    bytes32 slot = IMPLEMENTATION_SLOT;
    assembly {
        impl := sload(slot)
    }
}
```

## State Variable Reads

- **_admin** (`address`)
- **IMPLEMENTATION_SLOT** (`bytes32`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: BaseImmutableAdminUpgradeabilityProxy.admin() (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: external
  └─ [1] 🔒 MODIFIER: BaseImmutableAdminUpgradeabilityProxy.ifAdmin() (NodeID: 1)
      💬 Args: [no args]
    └─ [2] ⚙️ FUNCTION: Proxy._fallback() (NodeID: 2)
        💬 Args: [no args]
        👁️  Def: internal
      ├─ [3] ⚙️ FUNCTION: InitializableImmutableAdminUpgradeabilityProxy._willFallback() (NodeID: 3)
      │   💬 Args: [no args]
      │   👁️  Def: internal
      │ └─ [4] ⚙️ FUNCTION: BaseImmutableAdminUpgradeabilityProxy._willFallback() (NodeID: 4)
      │     💬 Args: [no args]
      │     👁️  Def: internal
      │   └─ [5] ⚙️ FUNCTION: Proxy._willFallback() (NodeID: 5)
      │       💬 Args: [no args]
      │       👁️  Def: internal
      └─ [3] ⚙️ FUNCTION: Proxy._delegate(address) (NodeID: 6)
          💬 Args: [_implementation()]
          👁️  Def: internal
        └─ [4] ⚙️ FUNCTION: BaseUpgradeabilityProxy._implementation() (NodeID: 7)
            💬 Args: [no args]
            👁️  Def: internal
```

## Documentation

### Function Documentation

 @notice Return the admin address
 @return The address of the proxy admin.
