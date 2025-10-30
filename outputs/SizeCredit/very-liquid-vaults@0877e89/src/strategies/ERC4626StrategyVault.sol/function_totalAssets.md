# Function: totalAssets()

**Contract**: [src/strategies/ERC4626StrategyVault.sol/contract_ERC4626StrategyVault.md]

## Metadata

- **Contract**: ERC4626StrategyVault
- **Signature**: `totalAssets()`
- **Visibility**: public
- **Source Range**: 4255:181:148

## Implementation

```solidity
/// @notice Returns the total assets managed by this strategy
///  @dev Converts the external vault shares held by this contract to asset value
///  @return The total assets under management
function totalAssets() virtual override(ERC4626Upgradeable, IERC4626) public view returns (uint256) {
    return vault().convertToAssets(vault().balanceOf(address(this)));
}
```

## Related Implementations

### vault()

- **Kind**: internal
- **Source**: 5486:112:148
- **Link**: `src/strategies/ERC4626StrategyVault.sol:ERC4626StrategyVault:vault()`

```solidity
/// @notice Returns the external vault
function vault() public view returns (IERC4626) {
    return _getERC4626StrategyVaultStorage()._vault;
}
```

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

## External Calls

- **IERC4626::convertToAssets(uint256)**
- **IERC4626::balanceOf(address)**

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: ERC4626StrategyVault.totalAssets() (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
  ├─ [1] ⚙️ FUNCTION: ERC4626StrategyVault.vault() (NodeID: 1)
  │   💬 Args: [no args]
  │   👁️  Def: public
  │ └─ [2] ⚙️ FUNCTION: ERC4626StrategyVault._getERC4626StrategyVaultStorage() (NodeID: 2)
  │     💬 Args: [no args]
  │     👁️  Def: private
  └─ [1] ⚙️ FUNCTION: ERC4626StrategyVault.vault() (NodeID: 3)
      💬 Args: [no args]
      👁️  Def: public
    └─ [2] ⚙️ FUNCTION: ERC4626StrategyVault._getERC4626StrategyVaultStorage() (NodeID: 4)
        💬 Args: [no args]
        👁️  Def: private
```

## Documentation

### Function Documentation

@notice Returns the total assets managed by this strategy
 @dev Converts the external vault shares held by this contract to asset value
 @return The total assets under management

### Interface Documentation

 @dev Returns the total amount of the underlying asset that is “managed” by Vault.
 - SHOULD include any compounding that occurs from yield.
 - MUST be inclusive of any fees that are charged against assets in the Vault.
 - MUST NOT revert.
