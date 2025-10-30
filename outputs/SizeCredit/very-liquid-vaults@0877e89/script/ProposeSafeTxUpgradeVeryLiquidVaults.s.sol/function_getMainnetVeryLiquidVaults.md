# Function: getMainnetVeryLiquidVaults()

**Contract**: [script/ProposeSafeTxUpgradeVeryLiquidVaults.s.sol/contract_ProposeSafeTxUpgradeVeryLiquidVaultsScript.md]

## Metadata

- **Contract**: ProposeSafeTxUpgradeVeryLiquidVaultsScript
- **Signature**: `getMainnetVeryLiquidVaults()`
- **Visibility**: public
- **Source Range**: 2442:268:138

## Implementation

```solidity
function getMainnetVeryLiquidVaults() public view returns (address[] memory ans) {
    ans = new address[](2);
    ans[0] = addresses[1][Contract.VeryLiquidVault_Core];
    ans[1] = addresses[1][Contract.VeryLiquidVault_Frontier];
    return ans;
}
```

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: ProposeSafeTxUpgradeVeryLiquidVaultsScript.getMainnetVeryLiquidVaults() (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
```
