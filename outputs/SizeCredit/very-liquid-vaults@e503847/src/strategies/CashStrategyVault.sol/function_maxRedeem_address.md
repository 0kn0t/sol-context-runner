# Function: maxRedeem(address)

**Contract**: [src/strategies/CashStrategyVault.sol/contract_CashStrategyVault.md]

## Metadata

- **Contract**: CashStrategyVault
- **Signature**: `maxRedeem(address)`
- **Visibility**: public
- **Source Range**: 8173:112:17
- **Inherited From**: ERC4626Upgradeable

## Implementation

```solidity
/// @dev See {IERC4626-maxRedeem}. 
function maxRedeem(address owner) virtual public view returns (uint256) {
    return balanceOf(owner);
}
```

## Related Implementations

### balanceOf(address)

- **Kind**: internal
- **Source**: 4087:171:15
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/token/ERC20/ERC20Upgradeable.sol:ERC20Upgradeable:balanceOf(address)`

```solidity
///  @dev See {IERC20-balanceOf}.
function balanceOf(address account) virtual public view returns (uint256) {
    ERC20Storage storage $ = _getERC20Storage();
    return $._balances[account];
}
```

### _getERC20Storage()

- **Kind**: internal
- **Source**: 1947:153:15
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/token/ERC20/ERC20Upgradeable.sol:ERC20Upgradeable:_getERC20Storage()`

```solidity
function _getERC20Storage() private pure returns (ERC20Storage storage $) {
    assembly {
        $.slot := ERC20StorageLocation
    }
}
```

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: ERC4626Upgradeable.maxRedeem(address) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
  └─ [1] ⚙️ FUNCTION: ERC20Upgradeable.balanceOf(address) (NodeID: 1)
      💬 Args: [owner]
      👁️  Def: public
    └─ [2] ⚙️ FUNCTION: ERC20Upgradeable._getERC20Storage() (NodeID: 2)
        💬 Args: [no args]
        👁️  Def: private
```

## Documentation

### Function Documentation

@dev See {IERC4626-maxRedeem}. 

### Interface Documentation

 @dev Returns the maximum amount of Vault shares that can be redeemed from the owner balance in the Vault,
 through a redeem call.
 - MUST return a limited value if owner is subject to some withdrawal limit or timelock.
 - MUST return balanceOf(owner) if owner is not subject to any withdrawal limit or timelock.
 - MUST NOT revert.
