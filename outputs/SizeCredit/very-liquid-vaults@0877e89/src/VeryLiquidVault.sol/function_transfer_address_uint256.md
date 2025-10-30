# Function: transfer(address,uint256)

**Contract**: [src/VeryLiquidVault.sol/contract_VeryLiquidVault.md]

## Metadata

- **Contract**: VeryLiquidVault
- **Signature**: `transfer(address,uint256)`
- **Visibility**: public
- **Source Range**: 4453:178:58
- **Inherited From**: ERC20Upgradeable

## Implementation

```solidity
///  @dev See {IERC20-transfer}.
///  Requirements:
///  - `to` cannot be the zero address.
///  - the caller must have a balance of at least `value`.
function transfer(address to, uint256 value) virtual public returns (bool) {
    address owner = _msgSender();
    _transfer(owner, to, value);
    return true;
}
```

## Call Tree

```
No call tree available
```

## Documentation

### Function Documentation

 @dev See {IERC20-transfer}.
 Requirements:
 - `to` cannot be the zero address.
 - the caller must have a balance of at least `value`.

### Interface Documentation

 @dev Moves a `value` amount of tokens from the caller's account to `to`.
 Returns a boolean value indicating whether the operation succeeded.
 Emits a {Transfer} event.
