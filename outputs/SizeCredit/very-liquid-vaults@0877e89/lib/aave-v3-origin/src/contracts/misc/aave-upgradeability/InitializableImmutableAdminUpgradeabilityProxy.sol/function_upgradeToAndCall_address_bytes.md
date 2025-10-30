# Function: upgradeToAndCall(address,bytes)

**Contract**: [lib/aave-v3-origin/src/contracts/misc/aave-upgradeability/InitializableImmutableAdminUpgradeabilityProxy.sol/contract_InitializableImmutableAdminUpgradeabilityProxy.md]

## Metadata

- **Contract**: InitializableImmutableAdminUpgradeabilityProxy
- **Signature**: `upgradeToAndCall(address,bytes)`
- **Visibility**: external
- **Source Range**: 2279:234:22
- **Inherited From**: BaseImmutableAdminUpgradeabilityProxy

## Implementation

```solidity
///  @notice Upgrade the backing implementation of the proxy and call a function
///  on the new implementation.
///  @dev This is useful to initialize the proxied contract.
///  @param newImplementation The address of the new implementation.
///  @param data Data to send as msg.data in the low level call.
///  It should include the signature and the parameters of the function to be called, as described in
///  https://solidity.readthedocs.io/en/v0.4.24/abi-spec.html#function-selector-and-argument-encoding.
function upgradeToAndCall(address newImplementation, bytes calldata data) external payable ifAdmin() {
    _upgradeTo(newImplementation);
    (bool success, ) = newImplementation.delegatecall(data);
    require(success);
}
```

## Related Implementations

### _upgradeTo(address)

- **Kind**: internal
- **Source**: 1335:142:7
- **Link**: `lib/aave-v3-origin/src/contracts/dependencies/openzeppelin/upgradeability/BaseUpgradeabilityProxy.sol:BaseUpgradeabilityProxy:_upgradeTo(address)`

```solidity
///  @dev Upgrades the proxy to a new implementation.
///  @param newImplementation Address of the new implementation.
function _upgradeTo(address newImplementation) internal {
    _setImplementation(newImplementation);
    emit Upgraded(newImplementation);
}
```

### _setImplementation(address)

- **Kind**: internal
- **Source**: 1614:334:7
- **Link**: `lib/aave-v3-origin/src/contracts/dependencies/openzeppelin/upgradeability/BaseUpgradeabilityProxy.sol:BaseUpgradeabilityProxy:_setImplementation(address)`

```solidity
///  @dev Sets the implementation address of the proxy.
///  @param newImplementation Address of the new implementation.
function _setImplementation(address newImplementation) internal {
    require(Address.isContract(newImplementation), "Cannot set a proxy implementation to a non-contract address");
    bytes32 slot = IMPLEMENTATION_SLOT;
    assembly {
        sstore(slot, newImplementation)
    }
}
```

### isContract(address)

- **Kind**: internal
- **Source**: 735:341:1
- **Link**: `lib/aave-v3-origin/src/contracts/dependencies/openzeppelin/contracts/Address.sol:Address:isContract(address)`

```solidity
///  @dev Returns true if `account` is a contract.
///  [IMPORTANT]
///  ====
///  It is unsafe to assume that an address for which this function returns
///  false is an externally-owned account (EOA) and not a contract.
///  Among others, `isContract` will return false for the following
///  types of addresses:
///   - an externally-owned account
///   - a contract in construction
///   - an address where a contract will be created
///   - an address where a contract lived, but was destroyed
///  ====
function isContract(address account) internal view returns (bool) {
    uint256 size;
    assembly {
        size := extcodesize(account)
    }
    return size > 0;
}
```

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

## External Calls

- **address::delegatecall(bytes calldata)**

## State Variable Reads

- **IMPLEMENTATION_SLOT** (`bytes32`)
- **_admin** (`address`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: BaseImmutableAdminUpgradeabilityProxy.upgradeToAndCall(address,bytes) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: external
  ├─ [1] ⚙️ FUNCTION: BaseUpgradeabilityProxy._upgradeTo(address) (NodeID: 1)
  │   💬 Args: [newImplementation]
  │   👁️  Def: internal
  │ └─ [2] ⚙️ FUNCTION: BaseUpgradeabilityProxy._setImplementation(address) (NodeID: 2)
  │     💬 Args: [newImplementation]
  │     👁️  Def: internal
  │   └─ [3] ⚙️ FUNCTION: Address.isContract(address) (NodeID: 3)
  │       💬 Args: [newImplementation]
  │       👁️  Def: internal
  └─ [1] 🔒 MODIFIER: BaseImmutableAdminUpgradeabilityProxy.ifAdmin() (NodeID: 4)
      💬 Args: [no args]
    └─ [2] ⚙️ FUNCTION: Proxy._fallback() (NodeID: 5)
        💬 Args: [no args]
        👁️  Def: internal
      ├─ [3] ⚙️ FUNCTION: InitializableImmutableAdminUpgradeabilityProxy._willFallback() (NodeID: 6)
      │   💬 Args: [no args]
      │   👁️  Def: internal
      │ └─ [4] ⚙️ FUNCTION: BaseImmutableAdminUpgradeabilityProxy._willFallback() (NodeID: 7)
      │     💬 Args: [no args]
      │     👁️  Def: internal
      │   └─ [5] ⚙️ FUNCTION: Proxy._willFallback() (NodeID: 8)
      │       💬 Args: [no args]
      │       👁️  Def: internal
      └─ [3] ⚙️ FUNCTION: Proxy._delegate(address) (NodeID: 9)
          💬 Args: [_implementation()]
          👁️  Def: internal
        └─ [4] ⚙️ FUNCTION: BaseUpgradeabilityProxy._implementation() (NodeID: 10)
            💬 Args: [no args]
            👁️  Def: internal
```

## Documentation

### Function Documentation

 @notice Upgrade the backing implementation of the proxy and call a function
 on the new implementation.
 @dev This is useful to initialize the proxied contract.
 @param newImplementation The address of the new implementation.
 @param data Data to send as msg.data in the low level call.
 It should include the signature and the parameters of the function to be called, as described in
 https://solidity.readthedocs.io/en/v0.4.24/abi-spec.html#function-selector-and-argument-encoding.
