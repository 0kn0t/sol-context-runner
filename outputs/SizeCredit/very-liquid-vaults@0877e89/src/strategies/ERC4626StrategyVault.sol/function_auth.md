# Function: auth()

**Contract**: [src/strategies/ERC4626StrategyVault.sol/contract_ERC4626StrategyVault.md]

## Metadata

- **Contract**: ERC4626StrategyVault
- **Signature**: `auth()`
- **Visibility**: public
- **Source Range**: 14480:104:149
- **Inherited From**: BaseVault

## Implementation

```solidity
/// @inheritdoc IVault
function auth() override public view returns (Auth) {
    return _getBaseVaultStorage()._auth;
}
```

## Related Implementations

### _getBaseVaultStorage()

- **Kind**: internal
- **Source**: 2552:165:149
- **Link**: `src/utils/BaseVault.sol:BaseVault:_getBaseVaultStorage()`

```solidity
function _getBaseVaultStorage() private pure returns (BaseVaultStorage storage $) {
    assembly {
        $.slot := BaseVaultStorageLocation
    }
}
```

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: BaseVault.auth() (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
  └─ [1] ⚙️ FUNCTION: BaseVault._getBaseVaultStorage() (NodeID: 1)
      💬 Args: [no args]
      👁️  Def: private
```

## Documentation

### Function Documentation

@inheritdoc IVault

### Interface Documentation

@notice Returns the address of the Auth contract used for permission checks
 @dev The Auth contract manages roles and access control for vault operations
 @dev External integrators can use this to query permissions but should not rely on its internal logic directly
