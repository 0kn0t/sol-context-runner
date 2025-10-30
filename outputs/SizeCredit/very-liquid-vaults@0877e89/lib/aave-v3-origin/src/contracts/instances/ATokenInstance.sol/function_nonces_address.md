# Function: nonces(address)

**Contract**: [lib/aave-v3-origin/src/contracts/instances/ATokenInstance.sol/contract_ATokenInstance.md]

## Metadata

- **Contract**: ATokenInstance
- **Signature**: `nonces(address)`
- **Visibility**: public
- **Source Range**: 1255:101:34
- **Inherited From**: EIP712Base

## Implementation

```solidity
///  @notice Returns the nonce value for address specified as parameter
///  @param owner The address for which the nonce is being returned
///  @return The nonce value for the input address`
function nonces(address owner) virtual public view returns (uint256) {
    return _nonces[owner];
}
```

## Call Tree

```
No call tree available
```

## Documentation

### Function Documentation

 @notice Returns the nonce value for address specified as parameter
 @param owner The address for which the nonce is being returned
 @return The nonce value for the input address`

### Interface Documentation

 @notice Returns the nonce for owner.
 @param owner The address of the owner
 @return The nonce of the owner
