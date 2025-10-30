# Function: getBaseVeryLiquidVaults()

**Contract**: [script/ProposeSafeTxUpgradeVeryLiquidVaults.s.sol/contract_ProposeSafeTxUpgradeVeryLiquidVaultsScript.md]

## Metadata

- **Contract**: ProposeSafeTxUpgradeVeryLiquidVaultsScript
- **Signature**: `getBaseVeryLiquidVaults()`
- **Visibility**: public
- **Source Range**: 2716:202:138

## Implementation

```solidity
function getBaseVeryLiquidVaults() public view returns (address[] memory ans) {
    ans = new address[](1);
    ans[0] = addresses[8453][Contract.VeryLiquidVault_Core];
    return ans;
}
```

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: ProposeSafeTxUpgradeVeryLiquidVaultsScript.getBaseVeryLiquidVaults() (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
```
