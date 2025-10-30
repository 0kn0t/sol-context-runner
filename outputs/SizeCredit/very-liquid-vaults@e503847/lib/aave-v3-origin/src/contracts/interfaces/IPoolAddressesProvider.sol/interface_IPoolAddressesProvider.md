# Interface: IPoolAddressesProvider

## Metadata

- **Name**: IPoolAddressesProvider
- **Type**: Interface
- **Path**: lib/aave-v3-origin/src/contracts/interfaces/IPoolAddressesProvider.sol
- **Documentation**:  @title IPoolAddressesProvider
   @author Aave
   @notice Defines the basic interface for a Pool Addresses Provider.

## Events

### MarketIdSet

```solidity
///  @dev Emitted when the market identifier is updated.
///  @param oldMarketId The old id of the market
///  @param newMarketId The new id of the market
event MarketIdSet(string indexed oldMarketId, string indexed newMarketId);
```

### PoolUpdated

```solidity
///  @dev Emitted when the pool is updated.
///  @param oldAddress The old address of the Pool
///  @param newAddress The new address of the Pool
event PoolUpdated(address indexed oldAddress, address indexed newAddress);
```

### PoolConfiguratorUpdated

```solidity
///  @dev Emitted when the pool configurator is updated.
///  @param oldAddress The old address of the PoolConfigurator
///  @param newAddress The new address of the PoolConfigurator
event PoolConfiguratorUpdated(address indexed oldAddress, address indexed newAddress);
```

### PriceOracleUpdated

```solidity
///  @dev Emitted when the price oracle is updated.
///  @param oldAddress The old address of the PriceOracle
///  @param newAddress The new address of the PriceOracle
event PriceOracleUpdated(address indexed oldAddress, address indexed newAddress);
```

### ACLManagerUpdated

```solidity
///  @dev Emitted when the ACL manager is updated.
///  @param oldAddress The old address of the ACLManager
///  @param newAddress The new address of the ACLManager
event ACLManagerUpdated(address indexed oldAddress, address indexed newAddress);
```

### ACLAdminUpdated

```solidity
///  @dev Emitted when the ACL admin is updated.
///  @param oldAddress The old address of the ACLAdmin
///  @param newAddress The new address of the ACLAdmin
event ACLAdminUpdated(address indexed oldAddress, address indexed newAddress);
```

### PriceOracleSentinelUpdated

```solidity
///  @dev Emitted when the price oracle sentinel is updated.
///  @param oldAddress The old address of the PriceOracleSentinel
///  @param newAddress The new address of the PriceOracleSentinel
event PriceOracleSentinelUpdated(address indexed oldAddress, address indexed newAddress);
```

### PoolDataProviderUpdated

```solidity
///  @dev Emitted when the pool data provider is updated.
///  @param oldAddress The old address of the PoolDataProvider
///  @param newAddress The new address of the PoolDataProvider
event PoolDataProviderUpdated(address indexed oldAddress, address indexed newAddress);
```

### ProxyCreated

```solidity
///  @dev Emitted when a new proxy is created.
///  @param id The identifier of the proxy
///  @param proxyAddress The address of the created proxy contract
///  @param implementationAddress The address of the implementation contract
event ProxyCreated(bytes32 indexed id, address indexed proxyAddress, address indexed implementationAddress);
```

### AddressSet

```solidity
///  @dev Emitted when a new non-proxied contract address is registered.
///  @param id The identifier of the contract
///  @param oldAddress The address of the old contract
///  @param newAddress The address of the new contract
event AddressSet(bytes32 indexed id, address indexed oldAddress, address indexed newAddress);
```

### AddressSetAsProxy

```solidity
///  @dev Emitted when the implementation of the proxy registered with id is updated
///  @param id The identifier of the contract
///  @param proxyAddress The address of the proxy contract
///  @param oldImplementationAddress The address of the old implementation contract
///  @param newImplementationAddress The address of the new implementation contract
event AddressSetAsProxy(bytes32 indexed id, address indexed proxyAddress, address oldImplementationAddress, address indexed newImplementationAddress);
```

## Public/External Functions

### getMarketId()

- **Signature**: `getMarketId()`
- **Visibility**: external
- **Source Range**: 3726:61:5

**Signature:**
```solidity
///  @notice Returns the id of the Aave market to which this contract points to.
///  @return The market id
function getMarketId() external view returns (string memory);;
```

### setMarketId(string)

- **Signature**: `setMarketId(string)`
- **Visibility**: external
- **Source Range**: 4046:59:5

**Signature:**
```solidity
///  @notice Associates an id with a specific PoolAddressesProvider.
///  @dev This can be used to create an onchain registry of PoolAddressesProviders to
///  identify and validate multiple Aave markets.
///  @param newMarketId The market id
function setMarketId(string calldata newMarketId) external;;
```

### getAddress(bytes32)

- **Signature**: `getAddress(bytes32)`
- **Visibility**: external
- **Source Range**: 4418:64:5

**Signature:**
```solidity
///  @notice Returns an address by its identifier.
///  @dev The returned address might be an EOA or a contract, potentially proxied
///  @dev It returns ZERO if there is no registered address with the given id
///  @param id The id
///  @return The address of the registered for the specified id
function getAddress(bytes32 id) external view returns (address);;
```

### setAddressAsProxy(bytes32,address)

- **Signature**: `setAddressAsProxy(bytes32,address)`
- **Visibility**: external
- **Source Range**: 4974:82:5

**Signature:**
```solidity
///  @notice General function to update the implementation of a proxy registered with
///  certain `id`. If there is no proxy registered, it will instantiate one and
///  set as implementation the `newImplementationAddress`.
///  @dev IMPORTANT Use this function carefully, only for ids that don't have an explicit
///  setter function, in order to avoid unexpected consequences
///  @param id The id
///  @param newImplementationAddress The address of the new implementation
function setAddressAsProxy(bytes32 id, address newImplementationAddress) external;;
```

### setAddress(bytes32,address)

- **Signature**: `setAddress(bytes32,address)`
- **Visibility**: external
- **Source Range**: 5307:61:5

**Signature:**
```solidity
///  @notice Sets an address for an id replacing the address saved in the addresses map.
///  @dev IMPORTANT Use this function carefully, as it will do a hard replacement
///  @param id The id
///  @param newAddress The address to set
function setAddress(bytes32 id, address newAddress) external;;
```

### getPool()

- **Signature**: `getPool()`
- **Visibility**: external
- **Source Range**: 5472:51:5

**Signature:**
```solidity
///  @notice Returns the address of the Pool proxy.
///  @return The Pool proxy address
function getPool() external view returns (address);;
```

### setPoolImpl(address)

- **Signature**: `setPoolImpl(address)`
- **Visibility**: external
- **Source Range**: 5754:51:5

**Signature:**
```solidity
///  @notice Updates the implementation of the Pool, or creates a proxy
///  setting the new `pool` implementation when the function is called for the first time.
///  @param newPoolImpl The new Pool implementation
function setPoolImpl(address newPoolImpl) external;;
```

### getPoolConfigurator()

- **Signature**: `getPoolConfigurator()`
- **Visibility**: external
- **Source Range**: 5933:63:5

**Signature:**
```solidity
///  @notice Returns the address of the PoolConfigurator proxy.
///  @return The PoolConfigurator proxy address
function getPoolConfigurator() external view returns (address);;
```

### setPoolConfiguratorImpl(address)

- **Signature**: `setPoolConfiguratorImpl(address)`
- **Visibility**: external
- **Source Range**: 6275:75:5

**Signature:**
```solidity
///  @notice Updates the implementation of the PoolConfigurator, or creates a proxy
///  setting the new `PoolConfigurator` implementation when the function is called for the first time.
///  @param newPoolConfiguratorImpl The new PoolConfigurator implementation
function setPoolConfiguratorImpl(address newPoolConfiguratorImpl) external;;
```

### getPriceOracle()

- **Signature**: `getPriceOracle()`
- **Visibility**: external
- **Source Range**: 6464:58:5

**Signature:**
```solidity
///  @notice Returns the address of the price oracle.
///  @return The address of the PriceOracle
function getPriceOracle() external view returns (address);;
```

### setPriceOracle(address)

- **Signature**: `setPriceOracle(address)`
- **Visibility**: external
- **Source Range**: 6654:57:5

**Signature:**
```solidity
///  @notice Updates the address of the price oracle.
///  @param newPriceOracle The address of the new PriceOracle
function setPriceOracle(address newPriceOracle) external;;
```

### getACLManager()

- **Signature**: `getACLManager()`
- **Visibility**: external
- **Source Range**: 6823:57:5

**Signature:**
```solidity
///  @notice Returns the address of the ACL manager.
///  @return The address of the ACLManager
function getACLManager() external view returns (address);;
```

### setACLManager(address)

- **Signature**: `setACLManager(address)`
- **Visibility**: external
- **Source Range**: 7009:55:5

**Signature:**
```solidity
///  @notice Updates the address of the ACL manager.
///  @param newAclManager The address of the new ACLManager
function setACLManager(address newAclManager) external;;
```

### getACLAdmin()

- **Signature**: `getACLAdmin()`
- **Visibility**: external
- **Source Range**: 7173:55:5

**Signature:**
```solidity
///  @notice Returns the address of the ACL admin.
///  @return The address of the ACL admin
function getACLAdmin() external view returns (address);;
```

### setACLAdmin(address)

- **Signature**: `setACLAdmin(address)`
- **Visibility**: external
- **Source Range**: 7352:51:5

**Signature:**
```solidity
///  @notice Updates the address of the ACL admin.
///  @param newAclAdmin The address of the new ACL admin
function setACLAdmin(address newAclAdmin) external;;
```

### getPriceOracleSentinel()

- **Signature**: `getPriceOracleSentinel()`
- **Visibility**: external
- **Source Range**: 7534:66:5

**Signature:**
```solidity
///  @notice Returns the address of the price oracle sentinel.
///  @return The address of the PriceOracleSentinel
function getPriceOracleSentinel() external view returns (address);;
```

### setPriceOracleSentinel(address)

- **Signature**: `setPriceOracleSentinel(address)`
- **Visibility**: external
- **Source Range**: 7757:73:5

**Signature:**
```solidity
///  @notice Updates the address of the price oracle sentinel.
///  @param newPriceOracleSentinel The address of the new PriceOracleSentinel
function setPriceOracleSentinel(address newPriceOracleSentinel) external;;
```

### getPoolDataProvider()

- **Signature**: `getPoolDataProvider()`
- **Visibility**: external
- **Source Range**: 7946:63:5

**Signature:**
```solidity
///  @notice Returns the address of the data provider.
///  @return The address of the DataProvider
function getPoolDataProvider() external view returns (address);;
```

### setPoolDataProvider(address)

- **Signature**: `setPoolDataProvider(address)`
- **Visibility**: external
- **Source Range**: 8144:63:5

**Signature:**
```solidity
///  @notice Updates the address of the data provider.
///  @param newDataProvider The address of the new DataProvider
function setPoolDataProvider(address newDataProvider) external;;
```
