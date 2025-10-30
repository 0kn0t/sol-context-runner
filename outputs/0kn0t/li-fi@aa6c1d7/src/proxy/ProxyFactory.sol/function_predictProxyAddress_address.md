# Function: predictProxyAddress(address)

**Contract**: [src/proxy/ProxyFactory.sol/contract_ProxyFactory.md]

## Metadata

- **Contract**: ProxyFactory
- **Signature**: `predictProxyAddress(address)`
- **Visibility**: external
- **Source Range**: 726:396:40

## Implementation

```solidity
///  @dev Calculate the deterministic address for a user's proxy before deployment
///  @param user The address of the user
///  @return The address the proxy would be deployed to
function predictProxyAddress(address user) external view returns (address) {
    bytes32 salt = keccak256(abi.encodePacked(user));
    bytes32 initCodeHash = keccak256(abi.encodePacked(type(MinimalProxy).creationCode, abi.encode(user, vmContract)));
    return address(uint160(uint256(keccak256(abi.encodePacked(bytes1(0xff), address(this), salt, initCodeHash)))));
}
```

## State Variable Reads

- **vmContract** (`address`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: ProxyFactory.predictProxyAddress(address) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: external
```

## Documentation

### Function Documentation

 @dev Calculate the deterministic address for a user's proxy before deployment
 @param user The address of the user
 @return The address the proxy would be deployed to
