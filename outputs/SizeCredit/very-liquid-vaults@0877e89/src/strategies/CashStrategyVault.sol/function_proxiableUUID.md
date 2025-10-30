# Function: proxiableUUID()

**Contract**: [src/strategies/CashStrategyVault.sol/contract_CashStrategyVault.md]

## Metadata

- **Contract**: CashStrategyVault
- **Signature**: `proxiableUUID()`
- **Visibility**: external
- **Source Range**: 3708:134:57
- **Inherited From**: UUPSUpgradeable

## Implementation

```solidity
///  @dev Implementation of the ERC-1822 {proxiableUUID} function. This returns the storage slot used by the
///  implementation. It is used to validate the implementation's compatibility when performing an upgrade.
///  IMPORTANT: A proxy pointing at a proxiable contract should not be considered proxiable itself, because this risks
///  bricking a proxy that upgrades to it, by delegating to itself until out of gas. Thus it is critical that this
///  function revert if invoked through a proxy. This is guaranteed by the `notDelegated` modifier.
function proxiableUUID() virtual external view notDelegated() returns (bytes32) {
    return ERC1967Utils.IMPLEMENTATION_SLOT;
}
```

## Related Implementations

### notDelegated()

- **Kind**: modifier
- **Source**: 2892:72:57
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/proxy/utils/UUPSUpgradeable.sol:UUPSUpgradeable:notDelegated()`

```solidity
///  @dev Check that the execution is not being performed through a delegate call. This allows a function to be
///  callable on the implementing contract but not through proxies.
modifier notDelegated() {
    _checkNotDelegated();
    _;
}
```

### _checkNotDelegated()

- **Kind**: internal
- **Source**: 5007:213:57
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/proxy/utils/UUPSUpgradeable.sol:UUPSUpgradeable:_checkNotDelegated()`

```solidity
///  @dev Reverts if the execution is performed via delegatecall.
///  See {notDelegated}.
function _checkNotDelegated() virtual internal view {
    if (address(this) != __self) {
        revert UUPSUnauthorizedCallContext();
    }
}
```

## State Variable Reads

- **__self** (`address`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: UUPSUpgradeable.proxiableUUID() (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: external
  └─ [1] 🔒 MODIFIER: UUPSUpgradeable.notDelegated() (NodeID: 1)
      💬 Args: [no args]
    └─ [2] ⚙️ FUNCTION: UUPSUpgradeable._checkNotDelegated() (NodeID: 2)
        💬 Args: [no args]
        👁️  Def: internal
```

## Documentation

### Function Documentation

 @dev Implementation of the ERC-1822 {proxiableUUID} function. This returns the storage slot used by the
 implementation. It is used to validate the implementation's compatibility when performing an upgrade.
 IMPORTANT: A proxy pointing at a proxiable contract should not be considered proxiable itself, because this risks
 bricking a proxy that upgrades to it, by delegating to itself until out of gas. Thus it is critical that this
 function revert if invoked through a proxy. This is guaranteed by the `notDelegated` modifier.

### Interface Documentation

 @dev Returns the storage slot that the proxiable contract assumes is being used to store the implementation
 address.
 IMPORTANT: A proxy pointing at a proxiable contract should not be considered proxiable itself, because this risks
 bricking a proxy that upgrades to it, by delegating to itself until out of gas. Thus it is critical that this
 function revert if invoked through a proxy.
