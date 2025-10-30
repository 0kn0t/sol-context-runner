# Function: pool()

**Contract**: [src/strategies/AaveStrategyVault.sol/contract_AaveStrategyVault.md]

## Metadata

- **Contract**: AaveStrategyVault
- **Signature**: `pool()`
- **Visibility**: public
- **Source Range**: 8500:104:146

## Implementation

```solidity
/// @notice Returns the Aave pool
///  @return The Aave pool
function pool() public view returns (IPool) {
    return _getAaveStrategyVaultStorage()._pool;
}
```

## Related Implementations

### _getAaveStrategyVaultStorage()

- **Kind**: internal
- **Source**: 2083:189:146
- **Link**: `src/strategies/AaveStrategyVault.sol:AaveStrategyVault:_getAaveStrategyVaultStorage()`

```solidity
function _getAaveStrategyVaultStorage() private pure returns (AaveStrategyVaultStorage storage $) {
    assembly {
        $.slot := AaveStrategyVaultStorageLocation
    }
}
```

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: AaveStrategyVault.pool() (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
  └─ [1] ⚙️ FUNCTION: AaveStrategyVault._getAaveStrategyVaultStorage() (NodeID: 1)
      💬 Args: [no args]
      👁️  Def: private
```

## Documentation

### Function Documentation

@notice Returns the Aave pool
 @return The Aave pool
