# Function: vault()

**Contract**: [src/strategies/ERC4626StrategyVault.sol/contract_ERC4626StrategyVault.md]

## Metadata

- **Contract**: ERC4626StrategyVault
- **Signature**: `vault()`
- **Visibility**: public
- **Source Range**: 5486:112:148

## Implementation

```solidity
/// @notice Returns the external vault
function vault() public view returns (IERC4626) {
    return _getERC4626StrategyVaultStorage()._vault;
}
```

## Related Implementations

### _getERC4626StrategyVaultStorage()

- **Kind**: internal
- **Source**: 1444:198:148
- **Link**: `src/strategies/ERC4626StrategyVault.sol:ERC4626StrategyVault:_getERC4626StrategyVaultStorage()`

```solidity
function _getERC4626StrategyVaultStorage() private pure returns (ERC4626StrategyVaultStorage storage $) {
    assembly {
        $.slot := ERC4626StrategyVaultStorageLocation
    }
}
```

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: ERC4626StrategyVault.vault() (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
  └─ [1] ⚙️ FUNCTION: ERC4626StrategyVault._getERC4626StrategyVaultStorage() (NodeID: 1)
      💬 Args: [no args]
      👁️  Def: private
```

## Documentation

### Function Documentation

@notice Returns the external vault
