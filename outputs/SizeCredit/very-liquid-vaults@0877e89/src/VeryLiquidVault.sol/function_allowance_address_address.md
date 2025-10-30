# Function: allowance(address,address)

**Contract**: [src/VeryLiquidVault.sol/contract_VeryLiquidVault.md]

## Metadata

- **Contract**: VeryLiquidVault
- **Signature**: `allowance(address,address)`
- **Visibility**: public
- **Source Range**: 4689:195:58
- **Inherited From**: ERC20Upgradeable

## Implementation

```solidity
///  @dev See {IERC20-allowance}.
function allowance(address owner, address spender) virtual public view returns (uint256) {
    ERC20Storage storage $ = _getERC20Storage();
    return $._allowances[owner][spender];
}
```

## Related Implementations

### _getERC20Storage()

- **Kind**: internal
- **Source**: 1947:153:58
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
┌─ [0] ⚙️ FUNCTION: ERC20Upgradeable.allowance(address,address) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
  └─ [1] ⚙️ FUNCTION: ERC20Upgradeable._getERC20Storage() (NodeID: 1)
      💬 Args: [no args]
      👁️  Def: private
```

## Documentation

### Function Documentation

 @dev See {IERC20-allowance}.

### Interface Documentation

 @dev Returns the remaining number of tokens that `spender` will be
 allowed to spend on behalf of `owner` through {transferFrom}. This is
 zero by default.
 This value changes when {approve} or {transferFrom} are called.
