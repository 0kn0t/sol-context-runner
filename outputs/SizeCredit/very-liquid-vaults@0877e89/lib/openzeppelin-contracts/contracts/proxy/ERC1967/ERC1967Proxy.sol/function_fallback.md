# Function: fallback()

**Contract**: [lib/openzeppelin-contracts/contracts/proxy/ERC1967/ERC1967Proxy.sol/contract_ERC1967Proxy.md]

## Metadata

- **Contract**: ERC1967Proxy
- **Signature**: `fallback()`
- **Visibility**: external
- **Source Range**: 2603:64:83
- **Inherited From**: Proxy

## Implementation

```solidity
///  @dev Fallback function that delegates calls to the address returned by `_implementation()`. Will run if no other
///  function in the contract matches the call data.
fallback() virtual external payable {
    _fallback();
}
```

## Related Implementations

### _fallback()

- **Kind**: internal
- **Source**: 2323:83:83
- **Link**: `lib/openzeppelin-contracts/contracts/proxy/Proxy.sol:Proxy:_fallback()`

```solidity
///  @dev Delegates the current call to the address returned by `_implementation()`.
///  This function does not return to its internal call site, it will return directly to the external caller.
function _fallback() virtual internal {
    _delegate(_implementation());
}
```

### _delegate(address)

- **Kind**: internal
- **Source**: 949:895:83
- **Link**: `lib/openzeppelin-contracts/contracts/proxy/Proxy.sol:Proxy:_delegate(address)`

```solidity
///  @dev Delegates the current call to `implementation`.
///  This function does not return to its internal call site, it will return directly to the external caller.
function _delegate(address implementation) virtual internal {
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
- **Source**: 1583:132:81
- **Link**: `lib/openzeppelin-contracts/contracts/proxy/ERC1967/ERC1967Proxy.sol:ERC1967Proxy:_implementation()`

```solidity
///  @dev Returns the current implementation address.
///  TIP: To get this value clients can read directly from the storage slot shown below (specified by ERC-1967) using
///  the https://eth.wiki/json-rpc/API#eth_getstorageat[`eth_getStorageAt`] RPC call.
///  `0x360894a13ba1a3210667c828492db98dca3e2076cc3735a920a3ca505d382bbc`
function _implementation() virtual override internal view returns (address) {
    return ERC1967Utils.getImplementation();
}
```

### getImplementation()

- **Kind**: internal
- **Source**: 1441:138:82
- **Link**: `lib/openzeppelin-contracts/contracts/proxy/ERC1967/ERC1967Utils.sol:ERC1967Utils:getImplementation()`

```solidity
///  @dev Returns the current implementation address.
function getImplementation() internal view returns (address) {
    return StorageSlot.getAddressSlot(IMPLEMENTATION_SLOT).value;
}
```

### getAddressSlot(bytes32)

- **Kind**: internal
- **Source**: 1899:163:103
- **Link**: `lib/openzeppelin-contracts/contracts/utils/StorageSlot.sol:StorageSlot:getAddressSlot(bytes32)`

```solidity
///  @dev Returns an `AddressSlot` with member `value` located at `slot`.
function getAddressSlot(bytes32 slot) internal pure returns (AddressSlot storage r) {
    assembly ("memory-safe") {
        r.slot := slot
    }
}
```

## State Variable Reads

- **IMPLEMENTATION_SLOT** (`bytes32`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: Proxy.fallback() (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: external
  └─ [1] ⚙️ FUNCTION: Proxy._fallback() (NodeID: 1)
      💬 Args: [no args]
      👁️  Def: internal
    └─ [2] ⚙️ FUNCTION: Proxy._delegate(address) (NodeID: 2)
        💬 Args: [_implementation()]
        👁️  Def: internal
      └─ [3] ⚙️ FUNCTION: ERC1967Proxy._implementation() (NodeID: 3)
          💬 Args: [no args]
          👁️  Def: internal
        └─ [4] ⚙️ FUNCTION: ERC1967Utils.getImplementation() (NodeID: 4)
            💬 Args: [no args]
            👁️  Def: internal
          └─ [5] ⚙️ FUNCTION: StorageSlot.getAddressSlot(bytes32) (NodeID: 5)
              💬 Args: [IMPLEMENTATION_SLOT]
              👁️  Def: internal
```

## Documentation

### Function Documentation

 @dev Fallback function that delegates calls to the address returned by `_implementation()`. Will run if no other
 function in the contract matches the call data.
