# Function: deployProxy(address)

**Contract**: [src/proxy/ProxyFactory.sol/contract_ProxyFactory.md]

## Metadata

- **Contract**: ProxyFactory
- **Signature**: `deployProxy(address)`
- **Visibility**: external
- **Source Range**: 1345:912:40

## Implementation

```solidity
///  @dev Deploy a new proxy for a specific address with deterministic address
///  @param user The address of the user to deploy a proxy for
///  @return The address of the newly deployed proxy
function deployProxy(address user) external returns (address) {
    if (user == address(0)) revert VmErrors.InvalidUserAddress();
    address proxyAddress = this.predictProxyAddress(user);
    if (proxyAddress.code.length != 0) revert VmErrors.ProxyAlreadyDeployed();
    bytes32 salt = keccak256(abi.encodePacked(user));
    MinimalProxy proxy = new MinimalProxy{salt: salt}(user, vmContract);
    emit ProxyDeployed(user, address(proxy));
    return address(proxy);
}
```

## External Calls

- **ProxyFactory::predictProxyAddress(address)**
- **unknown::unknown**

## State Variable Reads

- **vmContract** (`address`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: ProxyFactory.deployProxy(address) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: external
```

## Documentation

### Function Documentation

 @dev Deploy a new proxy for a specific address with deterministic address
 @param user The address of the user to deploy a proxy for
 @return The address of the newly deployed proxy
