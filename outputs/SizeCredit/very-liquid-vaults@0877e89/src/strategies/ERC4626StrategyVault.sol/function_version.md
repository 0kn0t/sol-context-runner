# Function: version()

**Contract**: [src/strategies/ERC4626StrategyVault.sol/contract_ERC4626StrategyVault.md]

## Metadata

- **Contract**: ERC4626StrategyVault
- **Signature**: `version()`
- **Visibility**: external
- **Source Range**: 15027:88:149
- **Inherited From**: BaseVault

## Implementation

```solidity
/// @notice Returns the version of the vault
///  @return The version of the vault
function version() external pure returns (string memory) {
    return VERSION;
}
```

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: BaseVault.version() (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: external
```

## Documentation

### Function Documentation

@notice Returns the version of the vault
 @return The version of the vault
