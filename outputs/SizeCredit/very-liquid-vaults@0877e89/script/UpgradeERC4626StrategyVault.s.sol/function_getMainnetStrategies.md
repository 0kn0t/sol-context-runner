# Function: getMainnetStrategies()

**Contract**: [script/UpgradeERC4626StrategyVault.s.sol/contract_UpgradeERC4626StrategyVaultScript.md]

## Metadata

- **Contract**: UpgradeERC4626StrategyVaultScript
- **Signature**: `getMainnetStrategies()`
- **Visibility**: public
- **Source Range**: 2405:375:140

## Implementation

```solidity
function getMainnetStrategies() public view returns (address[] memory ans) {
    ans = new address[](3);
    ans[0] = addresses[1][Contract.ERC4626StrategyVault_Morpho_Steakhouse];
    ans[1] = addresses[1][Contract.ERC4626StrategyVault_Morpho_Smokehouse];
    ans[2] = addresses[1][Contract.ERC4626StrategyVault_Morpho_MEV_Capital];
    return ans;
}
```

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: UpgradeERC4626StrategyVaultScript.getMainnetStrategies() (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
```
