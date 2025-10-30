# Function: aToken()

**Contract**: [src/strategies/AaveStrategyVault.sol/contract_AaveStrategyVault.md]

## Metadata

- **Contract**: AaveStrategyVault
- **Signature**: `aToken()`
- **Visibility**: public
- **Source Range**: 8682:110:146

## Implementation

```solidity
/// @notice Returns the Aave aToken
///  @return The Aave aToken
function aToken() public view returns (IAToken) {
    return _getAaveStrategyVaultStorage()._aToken;
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
┌─ [0] ⚙️ FUNCTION: AaveStrategyVault.aToken() (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
  └─ [1] ⚙️ FUNCTION: AaveStrategyVault._getAaveStrategyVaultStorage() (NodeID: 1)
      💬 Args: [no args]
      👁️  Def: private
```

## Documentation

### Function Documentation

@notice Returns the Aave aToken
 @return The Aave aToken
