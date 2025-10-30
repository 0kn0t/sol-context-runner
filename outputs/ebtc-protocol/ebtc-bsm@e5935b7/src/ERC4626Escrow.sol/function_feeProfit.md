# Function: feeProfit()

**Contract**: [src/ERC4626Escrow.sol/contract_ERC4626Escrow.md]

## Metadata

- **Contract**: ERC4626Escrow
- **Signature**: `feeProfit()`
- **Visibility**: public
- **Source Range**: 5939:253:22
- **Inherited From**: BaseEscrow

## Implementation

```solidity
/// @notice Calculates the profit generated from asset management
///  @return The amount of profit generated
function feeProfit() public view returns (uint256) {
    uint256 tb = _totalBalance();
    if (tb > totalAssetsDeposited) {
        unchecked {
            return tb - totalAssetsDeposited;
        }
    }
    return 0;
}
```

## Related Implementations

### _totalBalance()

- **Kind**: internal
- **Source**: 4995:275:39
- **Link**: `src/ERC4626Escrow.sol:ERC4626Escrow:_totalBalance()`

```solidity
/// @notice Returns the total balance of assets
///  @dev Combines local balance with the balance held in the external vault
///  @return The total balance of assets
function _totalBalance() override internal view returns (uint256) {
    /// @dev convertToAssets is the same as previewRedeem for OZ, Aave, Euler, Morpho
    return EXTERNAL_VAULT.convertToAssets(EXTERNAL_VAULT.balanceOf(address(this))) + super._totalBalance();
}
```

### _totalBalance()

- **Kind**: internal
- **Source**: 1706:125:22
- **Link**: `src/BaseEscrow.sol:BaseEscrow:_totalBalance()`

```solidity
/// @dev Internal function that checks the total balance of assets held by the contract
function _totalBalance() virtual internal view returns (uint256) {
    return ASSET_TOKEN.balanceOf(address(this));
}
```

## State Variable Reads

- **totalAssetsDeposited** (`uint256`)
- **EXTERNAL_VAULT** (`contract IERC4626`) [lib/openzeppelin-contracts/contracts/interfaces/IERC4626.sol/interface_IERC4626.md]
- **ASSET_TOKEN** (`contract IERC20`) [lib/openzeppelin-contracts/contracts/token/ERC20/IERC20.sol/interface_IERC20.md]

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: BaseEscrow.feeProfit() (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
  └─ [1] ⚙️ FUNCTION: ERC4626Escrow._totalBalance() (NodeID: 1)
      💬 Args: [no args]
      👁️  Def: internal
    └─ [2] ⚙️ FUNCTION: BaseEscrow._totalBalance() (NodeID: 2)
        💬 Args: [no args]
        👁️  Def: internal
```

## Documentation

### Function Documentation

@notice Calculates the profit generated from asset management
 @return The amount of profit generated
