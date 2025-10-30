# Function: supportsInterface(bytes4)

**Contract**: [src/Auth.sol/contract_Auth.md]

## Metadata

- **Contract**: Auth
- **Signature**: `supportsInterface(bytes4)`
- **Visibility**: public
- **Source Range**: 1035:146:67
- **Inherited From**: ERC165Upgradeable

## Implementation

```solidity
///  @dev See {IERC165-supportsInterface}.
function supportsInterface(bytes4 interfaceId) virtual public view returns (bool) {
    return interfaceId == type(IERC165).interfaceId;
}
```

## Call Tree

```
No call tree available
```

## Documentation

### Function Documentation

 @dev See {IERC165-supportsInterface}.

### Interface Documentation

 @dev Returns true if this contract implements the interface defined by
 `interfaceId`. See the corresponding
 https://eips.ethereum.org/EIPS/eip-165#how-interfaces-are-identified[ERC section]
 to learn more about how these ids are created.
 This function call must use less than 30 000 gas.
