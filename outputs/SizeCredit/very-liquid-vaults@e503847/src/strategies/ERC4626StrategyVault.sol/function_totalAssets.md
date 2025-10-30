# Function: totalAssets()

**Contract**: [src/strategies/ERC4626StrategyVault.sol/contract_ERC4626StrategyVault.md]

## Metadata

- **Contract**: ERC4626StrategyVault
- **Signature**: `totalAssets()`
- **Visibility**: public
- **Source Range**: 4606:177:62

## Implementation

```solidity
/// @notice Returns the total assets managed by this strategy
///  @dev Converts the external vault shares held by this contract to asset value
///  @return The total assets under management
function totalAssets() virtual override(ERC4626Upgradeable, IERC4626) public view returns (uint256) {
    return vault.convertToAssets(vault.balanceOf(address(this)));
}
```

## External Calls

- **IERC4626::convertToAssets(uint256)**
- **IERC4626::balanceOf(address)**

## State Variable Reads

- **vault** (`contract IERC4626`) [lib/openzeppelin-contracts/contracts/interfaces/IERC4626.sol/interface_IERC4626.md]

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: ERC4626StrategyVault.totalAssets() (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
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
