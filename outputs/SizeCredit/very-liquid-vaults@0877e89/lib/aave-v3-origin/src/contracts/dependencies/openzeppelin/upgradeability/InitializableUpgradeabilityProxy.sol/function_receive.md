# Function: receive()

**Contract**: [lib/aave-v3-origin/src/contracts/dependencies/openzeppelin/upgradeability/InitializableUpgradeabilityProxy.sol/contract_InitializableUpgradeabilityProxy.md]

## Metadata

- **Contract**: InitializableUpgradeabilityProxy
- **Signature**: `receive()`
- **Visibility**: external
- **Source Range**: 739:49:9
- **Inherited From**: Proxy

## Implementation

```solidity
///  @dev Fallback function that will run if call data is empty.
///  IMPORTANT. receive() on implementation contracts will be unreachable
receive() external payable {
    _fallback();
}
```

## Related Implementations

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

- **IMPLEMENTATION_SLOT** (`bytes32`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: Proxy.receive() (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: external
  └─ [1] ⚙️ FUNCTION: Proxy._fallback() (NodeID: 1)
      💬 Args: [no args]
      👁️  Def: internal
    ├─ [2] ⚙️ FUNCTION: Proxy._willFallback() (NodeID: 2)
    │   💬 Args: [no args]
    │   👁️  Def: internal
    └─ [2] ⚙️ FUNCTION: Proxy._delegate(address) (NodeID: 3)
        💬 Args: [_implementation()]
        👁️  Def: internal
      └─ [3] ⚙️ FUNCTION: BaseUpgradeabilityProxy._implementation() (NodeID: 4)
          💬 Args: [no args]
          👁️  Def: internal
```

## Documentation

### Function Documentation

 @dev Fallback function that will run if call data is empty.
 IMPORTANT. receive() on implementation contracts will be unreachable
