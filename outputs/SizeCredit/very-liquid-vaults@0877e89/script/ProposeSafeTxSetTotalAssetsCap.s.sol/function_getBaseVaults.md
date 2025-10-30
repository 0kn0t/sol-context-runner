# Function: getBaseVaults()

**Contract**: [script/ProposeSafeTxSetTotalAssetsCap.s.sol/contract_ProposeSafeTxSetTotalAssetsCapScript.md]

## Metadata

- **Contract**: ProposeSafeTxSetTotalAssetsCapScript
- **Signature**: `getBaseVaults()`
- **Visibility**: public
- **Source Range**: 2276:189:136

## Implementation

```solidity
function getBaseVaults() public view returns (address[] memory ans) {
    ans = new address[](1);
    ans[0] = addresses[8453][Contract.CashStrategyVault];
    return ans;
}
```

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: ProposeSafeTxSetTotalAssetsCapScript.getBaseVaults() (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
```
