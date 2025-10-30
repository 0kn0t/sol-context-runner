# Function: getBaseStrategies()

**Contract**: [script/UpgradeERC4626StrategyVault.s.sol/contract_UpgradeERC4626StrategyVaultScript.md]

## Metadata

- **Contract**: UpgradeERC4626StrategyVaultScript
- **Signature**: `getBaseStrategies()`
- **Visibility**: public
- **Source Range**: 2786:386:140

## Implementation

```solidity
function getBaseStrategies() public view returns (address[] memory ans) {
    ans = new address[](3);
    ans[0] = addresses[8453][Contract.ERC4626StrategyVault_Morpho_Gauntlet_Prime];
    ans[1] = addresses[8453][Contract.ERC4626StrategyVault_Morpho_Spark];
    ans[2] = addresses[8453][Contract.ERC4626StrategyVault_Morpho_Moonwell_Flagship];
    return ans;
}
```

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: UpgradeERC4626StrategyVaultScript.getBaseStrategies() (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
```
