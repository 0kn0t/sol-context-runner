# Function: setAddressAsProxy(bytes32,address)

**Contract**: [lib/aave-v3-origin/src/contracts/protocol/configuration/PoolAddressesProvider.sol/contract_PoolAddressesProvider.md]

## Metadata

- **Contract**: PoolAddressesProvider
- **Signature**: `setAddressAsProxy(bytes32,address)`
- **Visibility**: external
- **Source Range**: 2356:374:26

## Implementation

```solidity
/// @inheritdoc IPoolAddressesProvider
function setAddressAsProxy(bytes32 id, address newImplementationAddress) override external onlyOwner() {
    address proxyAddress = _addresses[id];
    address oldImplementationAddress = _getProxyImplementation(id);
    _updateImpl(id, newImplementationAddress);
    emit AddressSetAsProxy(id, proxyAddress, oldImplementationAddress, newImplementationAddress);
}
```

## Related Implementations

### _getProxyImplementation(bytes32)

- **Kind**: internal
- **Source**: 7925:368:26
- **Link**: `lib/aave-v3-origin/src/contracts/protocol/configuration/PoolAddressesProvider.sol:PoolAddressesProvider:_getProxyImplementation(bytes32)`

```solidity
///  @notice Returns the the implementation contract of the proxy contract by its identifier.
///  @dev It returns ZERO if there is no registered address with the given id
///  @dev It reverts if the registered address with the given id is not `InitializableImmutableAdminUpgradeabilityProxy`
///  @param id The id
///  @return The address of the implementation contract
function _getProxyImplementation(bytes32 id) internal returns (address) {
    address proxyAddress = _addresses[id];
    if (proxyAddress == address(0)) {
        return address(0);
    } else {
        address payable payableProxyAddress = payable(proxyAddress);
        return InitializableImmutableAdminUpgradeabilityProxy(payableProxyAddress).implementation();
    }
}
```

### _updateImpl(bytes32,address)

- **Kind**: internal
- **Source**: 6550:684:26
- **Link**: `lib/aave-v3-origin/src/contracts/protocol/configuration/PoolAddressesProvider.sol:PoolAddressesProvider:_updateImpl(bytes32,address)`

```solidity
///  @notice Internal function to update the implementation of a specific proxied component of the protocol.
///  @dev If there is no proxy registered with the given identifier, it creates the proxy setting `newAddress`
///    as implementation and calls the initialize() function on the proxy
///  @dev If there is already a proxy registered, it just updates the implementation to `newAddress` and
///    calls the initialize() function via upgradeToAndCall() in the proxy
///  @param id The id of the proxy to be updated
///  @param newAddress The address of the new implementation
function _updateImpl(bytes32 id, address newAddress) internal {
    address proxyAddress = _addresses[id];
    InitializableImmutableAdminUpgradeabilityProxy proxy;
    bytes memory params = abi.encodeWithSignature("initialize(address)", address(this));
    if (proxyAddress == address(0)) {
        proxy = new InitializableImmutableAdminUpgradeabilityProxy(address(this));
        _addresses[id] = proxyAddress = address(proxy);
        proxy.initialize(newAddress, params);
        emit ProxyCreated(id, proxyAddress, newAddress);
    } else {
        proxy = InitializableImmutableAdminUpgradeabilityProxy(payable(proxyAddress));
        proxy.upgradeToAndCall(newAddress, params);
    }
}
```

### onlyOwner()

- **Kind**: modifier
- **Source**: 1170:106:5
- **Link**: `lib/aave-v3-origin/src/contracts/dependencies/openzeppelin/contracts/Ownable.sol:Ownable:onlyOwner()`

```solidity
///  @dev Throws if called by any account other than the owner.
modifier onlyOwner() {
    require(_owner == _msgSender(), "Ownable: caller is not the owner");
    _;
}
```

### _msgSender()

- **Kind**: internal
- **Source**: 588:107:2
- **Link**: `lib/aave-v3-origin/src/contracts/dependencies/openzeppelin/contracts/Context.sol:Context:_msgSender()`

```solidity
function _msgSender() virtual internal view returns (address payable) {
    return payable(msg.sender);
}
```

## State Variable Reads

- **_addresses** (`mapping(bytes32 => address)`)
- **_owner** (`address`)

## State Variable Writes

- **_addresses** (`mapping(bytes32 => address)`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: PoolAddressesProvider.setAddressAsProxy(bytes32,address) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: external
  ├─ [1] ⚙️ FUNCTION: PoolAddressesProvider._getProxyImplementation(bytes32) (NodeID: 1)
  │   💬 Args: [id]
  │   👁️  Def: internal
  ├─ [1] ⚙️ FUNCTION: PoolAddressesProvider._updateImpl(bytes32,address) (NodeID: 2)
  │   💬 Args: [id, newImplementationAddress]
  │   👁️  Def: internal
  └─ [1] 🔒 MODIFIER: Ownable.onlyOwner() (NodeID: 3)
      💬 Args: [no args]
    └─ [2] ⚙️ FUNCTION: Context._msgSender() (NodeID: 4)
        💬 Args: [no args]
        👁️  Def: internal
```

## Documentation

### Function Documentation

@inheritdoc IPoolAddressesProvider

### Interface Documentation

 @notice General function to update the implementation of a proxy registered with
 certain `id`. If there is no proxy registered, it will instantiate one and
 set as implementation the `newImplementationAddress`.
 @dev IMPORTANT Use this function carefully, only for ids that don't have an explicit
 setter function, in order to avoid unexpected consequences
 @param id The id
 @param newImplementationAddress The address of the new implementation
