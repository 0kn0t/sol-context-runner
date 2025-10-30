# Contract: PoolAddressesProvider

## Metadata

- **Name**: PoolAddressesProvider
- **Type**: Contract
- **Path**: lib/aave-v3-origin/src/contracts/protocol/configuration/PoolAddressesProvider.sol
- **Documentation**:  @title PoolAddressesProvider
   @author Aave
   @notice Main registry of addresses part of or connected to the protocol, including permissioned roles
   @dev Acts as factory of proxies and admin of those, so with right to change its implementations
   @dev Owned by the Aave Governance

## Implements Interfaces

- **IPoolAddressesProvider** [lib/aave-v3-origin/src/contracts/interfaces/IPoolAddressesProvider.sol/interface_IPoolAddressesProvider.md]

## State Variables

### _owner (inherited from Ownable)

```solidity
address private _owner
```

### _marketId

```solidity
string private _marketId
```

### _addresses

```solidity
mapping(bytes32 => address) private _addresses
```

### POOL

```solidity
bytes32 private constant POOL = "POOL"
```

### POOL_CONFIGURATOR

```solidity
bytes32 private constant POOL_CONFIGURATOR = "POOL_CONFIGURATOR"
```

### PRICE_ORACLE

```solidity
bytes32 private constant PRICE_ORACLE = "PRICE_ORACLE"
```

### ACL_MANAGER

```solidity
bytes32 private constant ACL_MANAGER = "ACL_MANAGER"
```

### ACL_ADMIN

```solidity
bytes32 private constant ACL_ADMIN = "ACL_ADMIN"
```

### PRICE_ORACLE_SENTINEL

```solidity
bytes32 private constant PRICE_ORACLE_SENTINEL = "PRICE_ORACLE_SENTINEL"
```

### DATA_PROVIDER

```solidity
bytes32 private constant DATA_PROVIDER = "DATA_PROVIDER"
```

## Events

### OwnershipTransferred (inherited from Ownable)

```solidity
event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);
```

### MarketIdSet (inherited from IPoolAddressesProvider)

```solidity
///  @dev Emitted when the market identifier is updated.
///  @param oldMarketId The old id of the market
///  @param newMarketId The new id of the market
event MarketIdSet(string indexed oldMarketId, string indexed newMarketId);
```

### PoolUpdated (inherited from IPoolAddressesProvider)

```solidity
///  @dev Emitted when the pool is updated.
///  @param oldAddress The old address of the Pool
///  @param newAddress The new address of the Pool
event PoolUpdated(address indexed oldAddress, address indexed newAddress);
```

### PoolConfiguratorUpdated (inherited from IPoolAddressesProvider)

```solidity
///  @dev Emitted when the pool configurator is updated.
///  @param oldAddress The old address of the PoolConfigurator
///  @param newAddress The new address of the PoolConfigurator
event PoolConfiguratorUpdated(address indexed oldAddress, address indexed newAddress);
```

### PriceOracleUpdated (inherited from IPoolAddressesProvider)

```solidity
///  @dev Emitted when the price oracle is updated.
///  @param oldAddress The old address of the PriceOracle
///  @param newAddress The new address of the PriceOracle
event PriceOracleUpdated(address indexed oldAddress, address indexed newAddress);
```

### ACLManagerUpdated (inherited from IPoolAddressesProvider)

```solidity
///  @dev Emitted when the ACL manager is updated.
///  @param oldAddress The old address of the ACLManager
///  @param newAddress The new address of the ACLManager
event ACLManagerUpdated(address indexed oldAddress, address indexed newAddress);
```

### ACLAdminUpdated (inherited from IPoolAddressesProvider)

```solidity
///  @dev Emitted when the ACL admin is updated.
///  @param oldAddress The old address of the ACLAdmin
///  @param newAddress The new address of the ACLAdmin
event ACLAdminUpdated(address indexed oldAddress, address indexed newAddress);
```

### PriceOracleSentinelUpdated (inherited from IPoolAddressesProvider)

```solidity
///  @dev Emitted when the price oracle sentinel is updated.
///  @param oldAddress The old address of the PriceOracleSentinel
///  @param newAddress The new address of the PriceOracleSentinel
event PriceOracleSentinelUpdated(address indexed oldAddress, address indexed newAddress);
```

### PoolDataProviderUpdated (inherited from IPoolAddressesProvider)

```solidity
///  @dev Emitted when the pool data provider is updated.
///  @param oldAddress The old address of the PoolDataProvider
///  @param newAddress The new address of the PoolDataProvider
event PoolDataProviderUpdated(address indexed oldAddress, address indexed newAddress);
```

### ProxyCreated (inherited from IPoolAddressesProvider)

```solidity
///  @dev Emitted when a new proxy is created.
///  @param id The identifier of the proxy
///  @param proxyAddress The address of the created proxy contract
///  @param implementationAddress The address of the implementation contract
event ProxyCreated(bytes32 indexed id, address indexed proxyAddress, address indexed implementationAddress);
```

### AddressSet (inherited from IPoolAddressesProvider)

```solidity
///  @dev Emitted when a new non-proxied contract address is registered.
///  @param id The identifier of the contract
///  @param oldAddress The address of the old contract
///  @param newAddress The address of the new contract
event AddressSet(bytes32 indexed id, address indexed oldAddress, address indexed newAddress);
```

### AddressSetAsProxy (inherited from IPoolAddressesProvider)

```solidity
///  @dev Emitted when the implementation of the proxy registered with id is updated
///  @param id The identifier of the contract
///  @param proxyAddress The address of the proxy contract
///  @param oldImplementationAddress The address of the old implementation contract
///  @param newImplementationAddress The address of the new implementation contract
event AddressSetAsProxy(bytes32 indexed id, address indexed proxyAddress, address oldImplementationAddress, address indexed newImplementationAddress);
```

## Public/External Functions

### constructor(string,address)

- **Signature**: `constructor(string,address)`
- **Visibility**: public
- **Source Range**: 1497:114:26
- **Details**: [function_constructor_string_address.md](./function_constructor_string_address.md)

**Signature:**
```solidity
///  @dev Constructor.
///  @param marketId The identifier of the market.
///  @param owner The owner address of this contract.
constructor(string memory marketId, address owner);
```

### getMarketId()

- **Signature**: `getMarketId()`
- **Visibility**: external
- **Source Range**: 1656:97:26
- **Details**: [function_getMarketId.md](./function_getMarketId.md)

**Signature:**
```solidity
/// @inheritdoc IPoolAddressesProvider
function getMarketId() override external view returns (string memory);
```

### setMarketId(string)

- **Signature**: `setMarketId(string)`
- **Visibility**: external
- **Source Range**: 1798:112:26
- **Details**: [function_setMarketId_string.md](./function_setMarketId_string.md)

**Signature:**
```solidity
/// @inheritdoc IPoolAddressesProvider
function setMarketId(string memory newMarketId) override external onlyOwner();
```

### getAddress(bytes32)

- **Signature**: `getAddress(bytes32)`
- **Visibility**: public
- **Source Range**: 1955:103:26
- **Details**: [function_getAddress_bytes32.md](./function_getAddress_bytes32.md)

**Signature:**
```solidity
/// @inheritdoc IPoolAddressesProvider
function getAddress(bytes32 id) override public view returns (address);
```

### setAddress(bytes32,address)

- **Signature**: `setAddress(bytes32,address)`
- **Visibility**: external
- **Source Range**: 2103:208:26
- **Details**: [function_setAddress_bytes32_address.md](./function_setAddress_bytes32_address.md)

**Signature:**
```solidity
/// @inheritdoc IPoolAddressesProvider
function setAddress(bytes32 id, address newAddress) override external onlyOwner();
```

### setAddressAsProxy(bytes32,address)

- **Signature**: `setAddressAsProxy(bytes32,address)`
- **Visibility**: external
- **Source Range**: 2356:374:26
- **Details**: [function_setAddressAsProxy_bytes32_address.md](./function_setAddressAsProxy_bytes32_address.md)

**Signature:**
```solidity
/// @inheritdoc IPoolAddressesProvider
function setAddressAsProxy(bytes32 id, address newImplementationAddress) override external onlyOwner();
```

### getPool()

- **Signature**: `getPool()`
- **Visibility**: external
- **Source Range**: 2775:94:26
- **Details**: [function_getPool.md](./function_getPool.md)

**Signature:**
```solidity
/// @inheritdoc IPoolAddressesProvider
function getPool() override external view returns (address);
```

### setPoolImpl(address)

- **Signature**: `setPoolImpl(address)`
- **Visibility**: external
- **Source Range**: 2914:216:26
- **Details**: [function_setPoolImpl_address.md](./function_setPoolImpl_address.md)

**Signature:**
```solidity
/// @inheritdoc IPoolAddressesProvider
function setPoolImpl(address newPoolImpl) override external onlyOwner();
```

### getPoolConfigurator()

- **Signature**: `getPoolConfigurator()`
- **Visibility**: external
- **Source Range**: 3175:119:26
- **Details**: [function_getPoolConfigurator.md](./function_getPoolConfigurator.md)

**Signature:**
```solidity
/// @inheritdoc IPoolAddressesProvider
function getPoolConfigurator() override external view returns (address);
```

### setPoolConfiguratorImpl(address)

- **Signature**: `setPoolConfiguratorImpl(address)`
- **Visibility**: external
- **Source Range**: 3339:326:26
- **Details**: [function_setPoolConfiguratorImpl_address.md](./function_setPoolConfiguratorImpl_address.md)

**Signature:**
```solidity
/// @inheritdoc IPoolAddressesProvider
function setPoolConfiguratorImpl(address newPoolConfiguratorImpl) override external onlyOwner();
```

### getPriceOracle()

- **Signature**: `getPriceOracle()`
- **Visibility**: external
- **Source Range**: 3710:109:26
- **Details**: [function_getPriceOracle.md](./function_getPriceOracle.md)

**Signature:**
```solidity
/// @inheritdoc IPoolAddressesProvider
function getPriceOracle() override external view returns (address);
```

### setPriceOracle(address)

- **Signature**: `setPriceOracle(address)`
- **Visibility**: external
- **Source Range**: 3864:244:26
- **Details**: [function_setPriceOracle_address.md](./function_setPriceOracle_address.md)

**Signature:**
```solidity
/// @inheritdoc IPoolAddressesProvider
function setPriceOracle(address newPriceOracle) override external onlyOwner();
```

### getACLManager()

- **Signature**: `getACLManager()`
- **Visibility**: external
- **Source Range**: 4153:107:26
- **Details**: [function_getACLManager.md](./function_getACLManager.md)

**Signature:**
```solidity
/// @inheritdoc IPoolAddressesProvider
function getACLManager() override external view returns (address);
```

### setACLManager(address)

- **Signature**: `setACLManager(address)`
- **Visibility**: external
- **Source Range**: 4305:235:26
- **Details**: [function_setACLManager_address.md](./function_setACLManager_address.md)

**Signature:**
```solidity
/// @inheritdoc IPoolAddressesProvider
function setACLManager(address newAclManager) override external onlyOwner();
```

### getACLAdmin()

- **Signature**: `getACLAdmin()`
- **Visibility**: external
- **Source Range**: 4585:103:26
- **Details**: [function_getACLAdmin.md](./function_getACLAdmin.md)

**Signature:**
```solidity
/// @inheritdoc IPoolAddressesProvider
function getACLAdmin() override external view returns (address);
```

### setACLAdmin(address)

- **Signature**: `setACLAdmin(address)`
- **Visibility**: external
- **Source Range**: 4733:217:26
- **Details**: [function_setACLAdmin_address.md](./function_setACLAdmin_address.md)

**Signature:**
```solidity
/// @inheritdoc IPoolAddressesProvider
function setACLAdmin(address newAclAdmin) override external onlyOwner();
```

### getPriceOracleSentinel()

- **Signature**: `getPriceOracleSentinel()`
- **Visibility**: external
- **Source Range**: 4995:126:26
- **Details**: [function_getPriceOracleSentinel.md](./function_getPriceOracleSentinel.md)

**Signature:**
```solidity
/// @inheritdoc IPoolAddressesProvider
function getPriceOracleSentinel() override external view returns (address);
```

### setPriceOracleSentinel(address)

- **Signature**: `setPriceOracleSentinel(address)`
- **Visibility**: external
- **Source Range**: 5166:318:26
- **Details**: [function_setPriceOracleSentinel_address.md](./function_setPriceOracleSentinel_address.md)

**Signature:**
```solidity
/// @inheritdoc IPoolAddressesProvider
function setPriceOracleSentinel(address newPriceOracleSentinel) override external onlyOwner();
```

### getPoolDataProvider()

- **Signature**: `getPoolDataProvider()`
- **Visibility**: external
- **Source Range**: 5529:115:26
- **Details**: [function_getPoolDataProvider.md](./function_getPoolDataProvider.md)

**Signature:**
```solidity
/// @inheritdoc IPoolAddressesProvider
function getPoolDataProvider() override external view returns (address);
```

### setPoolDataProvider(address)

- **Signature**: `setPoolDataProvider(address)`
- **Visibility**: external
- **Source Range**: 5689:261:26
- **Details**: [function_setPoolDataProvider_address.md](./function_setPoolDataProvider_address.md)

**Signature:**
```solidity
/// @inheritdoc IPoolAddressesProvider
function setPoolDataProvider(address newDataProvider) override external onlyOwner();
```

### constructor() (inherited from Ownable)

- **Signature**: `constructor()`
- **Visibility**: public
- **Source Range**: 816:135:5
- **Details**: [function_constructor.md](./function_constructor.md)

**Signature:**
```solidity
///  @dev Initializes the contract setting the deployer as the initial owner.
constructor();
```

### owner() (inherited from Ownable)

- **Signature**: `owner()`
- **Visibility**: public
- **Source Range**: 1019:71:5
- **Details**: [function_owner.md](./function_owner.md)

**Signature:**
```solidity
///  @dev Returns the address of the current owner.
function owner() public view returns (address);
```

### renounceOwnership() (inherited from Ownable)

- **Signature**: `renounceOwnership()`
- **Visibility**: public
- **Source Range**: 1602:135:5
- **Details**: [function_renounceOwnership.md](./function_renounceOwnership.md)

**Signature:**
```solidity
///  @dev Leaves the contract without owner. It will not be possible to call
///  `onlyOwner` functions anymore. Can only be called by the current owner.
///  NOTE: Renouncing ownership will leave the contract without an owner,
///  thereby removing any functionality that is only available to the owner.
function renounceOwnership() virtual public onlyOwner();
```

### transferOwnership(address) (inherited from Ownable)

- **Signature**: `transferOwnership(address)`
- **Visibility**: public
- **Source Range**: 1876:226:5
- **Details**: [function_transferOwnership_address.md](./function_transferOwnership_address.md)

**Signature:**
```solidity
///  @dev Transfers ownership of the contract to a new account (`newOwner`).
///  Can only be called by the current owner.
function transferOwnership(address newOwner) virtual public onlyOwner();
```
