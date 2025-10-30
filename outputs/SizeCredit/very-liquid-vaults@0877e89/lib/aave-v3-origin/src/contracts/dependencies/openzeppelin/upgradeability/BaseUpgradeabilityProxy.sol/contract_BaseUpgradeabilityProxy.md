# Contract: BaseUpgradeabilityProxy

## Metadata

- **Name**: BaseUpgradeabilityProxy
- **Type**: Contract
- **Path**: lib/aave-v3-origin/src/contracts/dependencies/openzeppelin/upgradeability/BaseUpgradeabilityProxy.sol
- **Documentation**:  @title BaseUpgradeabilityProxy
   @dev This contract implements a proxy that allows to change the
   implementation address to which it will delegate.
   Such a change is called an implementation upgrade.

## State Variables

### IMPLEMENTATION_SLOT

```solidity
///  @dev Storage slot with the address of the current implementation.
///  This is the keccak-256 hash of "eip1967.proxy.implementation" subtracted by 1, and is
///  validated in the constructor.
bytes32 internal constant IMPLEMENTATION_SLOT = 0x360894a13ba1a3210667c828492db98dca3e2076cc3735a920a3ca505d382bbc
```

## Events

### Upgraded

```solidity
///  @dev Emitted when the implementation is upgraded.
///  @param implementation Address of the new implementation.
event Upgraded(address indexed implementation);
```

## Public/External Functions

### fallback() (inherited from Proxy)

- **Signature**: `fallback()`
- **Visibility**: external
- **Source Range**: 534:50:9
- **Details**: [function_fallback.md](./function_fallback.md)

**Signature:**
```solidity
///  @dev Fallback function.
///  Will run if no other function in the contract matches the call data.
///  Implemented entirely in `_fallback`.
fallback() external payable;
```

### receive() (inherited from Proxy)

- **Signature**: `receive()`
- **Visibility**: external
- **Source Range**: 739:49:9
- **Details**: [function_receive.md](./function_receive.md)

**Signature:**
```solidity
///  @dev Fallback function that will run if call data is empty.
///  IMPORTANT. receive() on implementation contracts will be unreachable
receive() external payable;
```
