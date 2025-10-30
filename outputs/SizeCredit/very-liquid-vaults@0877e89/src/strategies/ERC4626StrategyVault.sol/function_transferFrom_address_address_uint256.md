# Function: transferFrom(address,address,uint256)

**Contract**: [src/strategies/ERC4626StrategyVault.sol/contract_ERC4626StrategyVault.md]

## Metadata

- **Contract**: ERC4626StrategyVault
- **Signature**: `transferFrom(address,address,uint256)`
- **Visibility**: public
- **Source Range**: 5969:244:58
- **Inherited From**: ERC20Upgradeable

## Implementation

```solidity
///  @dev See {IERC20-transferFrom}.
///  Skips emitting an {Approval} event indicating an allowance update. This is not
///  required by the ERC. See {xref-ERC20-_approve-address-address-uint256-bool-}[_approve].
///  NOTE: Does not update the allowance if the current allowance
///  is the maximum `uint256`.
///  Requirements:
///  - `from` and `to` cannot be the zero address.
///  - `from` must have a balance of at least `value`.
///  - the caller must have allowance for ``from``'s tokens of at least
///  `value`.
function transferFrom(address from, address to, uint256 value) virtual public returns (bool) {
    address spender = _msgSender();
    _spendAllowance(from, spender, value);
    _transfer(from, to, value);
    return true;
}
```

## Call Tree

```
No call tree available
```

## Documentation

### Function Documentation

 @dev See {IERC20-transferFrom}.
 Skips emitting an {Approval} event indicating an allowance update. This is not
 required by the ERC. See {xref-ERC20-_approve-address-address-uint256-bool-}[_approve].
 NOTE: Does not update the allowance if the current allowance
 is the maximum `uint256`.
 Requirements:
 - `from` and `to` cannot be the zero address.
 - `from` must have a balance of at least `value`.
 - the caller must have allowance for ``from``'s tokens of at least
 `value`.

### Interface Documentation

 @dev Moves a `value` amount of tokens from `from` to `to` using the
 allowance mechanism. `value` is then deducted from the caller's
 allowance.
 Returns a boolean value indicating whether the operation succeeded.
 Emits a {Transfer} event.
