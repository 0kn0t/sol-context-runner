# Function: DOMAIN_SEPARATOR()

**Contract**: [lib/aave-v3-origin/src/contracts/instances/ATokenInstance.sol/contract_ATokenInstance.md]

## Metadata

- **Contract**: ATokenInstance
- **Signature**: `DOMAIN_SEPARATOR()`
- **Visibility**: public
- **Source Range**: 862:185:34
- **Inherited From**: EIP712Base

## Implementation

```solidity
///  @notice Get the domain separator for the token
///  @dev Return cached value if chainId matches cache, otherwise recomputes separator
///  @return The domain separator of the token at current chain
function DOMAIN_SEPARATOR() virtual public view returns (bytes32) {
    if (block.chainid == _chainId) {
        return _domainSeparator;
    }
    return _calculateDomainSeparator();
}
```

## Call Tree

```
No call tree available
```

## Documentation

### Function Documentation

 @notice Get the domain separator for the token
 @dev Return cached value if chainId matches cache, otherwise recomputes separator
 @return The domain separator of the token at current chain

### Interface Documentation

 @notice Get the domain separator for the token
 @dev Return cached value if chainId matches cache, otherwise recomputes separator
 @return The domain separator of the token at current chain
