# Function: totalAssets()

**Contract**: [src/VeryLiquidVault.sol/contract_VeryLiquidVault.md]

## Metadata

- **Contract**: VeryLiquidVault
- **Signature**: `totalAssets()`
- **Visibility**: public
- **Source Range**: 7057:583:145

## Implementation

```solidity
/// @inheritdoc ERC4626Upgradeable
///  @dev The total assets is the sum of the assets in all strategies
function totalAssets() virtual override(ERC4626Upgradeable, IERC4626) public view returns (uint256 total) {
    VeryLiquidVaultStorage storage $ = _getVeryLiquidVaultStorage();
    uint256 length = $._strategies.length;
    for (uint256 i = 0; i < length; ++i) {
        IVault strategy = $._strategies[i];
        uint256 strategyBalance = strategy.balanceOf(address(this));
        if (strategyBalance == 0) continue;
        total += strategy.convertToAssets(strategyBalance);
    }
}
```

## Related Implementations

### _getVeryLiquidVaultStorage()

- **Kind**: internal
- **Source**: 1874:183:145
- **Link**: `src/VeryLiquidVault.sol:VeryLiquidVault:_getVeryLiquidVaultStorage()`

```solidity
function _getVeryLiquidVaultStorage() private pure returns (VeryLiquidVaultStorage storage $) {
    assembly {
        $.slot := VeryLiquidVaultStorageLocation
    }
}
```

## External Calls

- **IVault::balanceOf(address)**
- **IVault::convertToAssets(uint256)**

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: VeryLiquidVault.totalAssets() (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
  └─ [1] ⚙️ FUNCTION: VeryLiquidVault._getVeryLiquidVaultStorage() (NodeID: 1)
      💬 Args: [no args]
      👁️  Def: private
```

## Documentation

### Function Documentation

@inheritdoc ERC4626Upgradeable
 @dev The total assets is the sum of the assets in all strategies

### Interface Documentation

 @dev Returns the total amount of the underlying asset that is “managed” by Vault.
 - SHOULD include any compounding that occurs from yield.
 - MUST be inclusive of any fees that are charged against assets in the Vault.
 - MUST NOT revert.
