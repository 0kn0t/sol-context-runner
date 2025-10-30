# Contract: ProxyFactory

## Metadata

- **Name**: ProxyFactory
- **Type**: Contract
- **Path**: src/proxy/ProxyFactory.sol
- **Documentation**:  @title Proxy Factory
   @dev Contract to create and manage proxies

## State Variables

### vmContract

```solidity
address public immutable vmContract
```

## Events

### ProxyDeployed

```solidity
event ProxyDeployed(address indexed user, address indexed proxy);
```

## Public/External Functions

### constructor(address)

- **Signature**: `constructor(address)`
- **Visibility**: public
- **Source Range**: 444:74:40
- **Details**: [function_constructor_address.md](./function_constructor_address.md)

**Signature:**
```solidity
constructor(address _vmContract);
```

### predictProxyAddress(address)

- **Signature**: `predictProxyAddress(address)`
- **Visibility**: external
- **Source Range**: 726:396:40
- **Details**: [function_predictProxyAddress_address.md](./function_predictProxyAddress_address.md)

**Signature:**
```solidity
///  @dev Calculate the deterministic address for a user's proxy before deployment
///  @param user The address of the user
///  @return The address the proxy would be deployed to
function predictProxyAddress(address user) external view returns (address);
```

### deployProxy(address)

- **Signature**: `deployProxy(address)`
- **Visibility**: external
- **Source Range**: 1345:912:40
- **Details**: [function_deployProxy_address.md](./function_deployProxy_address.md)

**Signature:**
```solidity
///  @dev Deploy a new proxy for a specific address with deterministic address
///  @param user The address of the user to deploy a proxy for
///  @return The address of the newly deployed proxy
function deployProxy(address user) external returns (address);
```
