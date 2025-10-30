# Function: nonces(address)

**Contract**: [src/strategies/AaveStrategyVault.sol/contract_AaveStrategyVault.md]

## Metadata

- **Contract**: AaveStrategyVault
- **Signature**: `nonces(address)`
- **Visibility**: public
- **Source Range**: 1259:164:63
- **Inherited From**: NoncesUpgradeable

## Implementation

```solidity
///  @dev Returns the next unused nonce for an address.
function nonces(address owner) virtual public view returns (uint256) {
    NoncesStorage storage $ = _getNoncesStorage();
    return $._nonces[owner];
}
```

## Call Tree

```
No call tree available
```

## Documentation

### Function Documentation

 @dev Returns the next unused nonce for an address.

### Interface Documentation

 @dev Returns the current nonce for `owner`. This value must be
 included whenever a signature is generated for {permit}.
 Every successful call to {permit} increases ``owner``'s nonce by one. This
 prevents a signature from being used multiple times.
