# Function: getMainnetVaults()

**Contract**: [script/ProposeSafeTxSetTotalAssetsCap.s.sol/contract_ProposeSafeTxSetTotalAssetsCapScript.md]

## Metadata

- **Contract**: ProposeSafeTxSetTotalAssetsCapScript
- **Signature**: `getMainnetVaults()`
- **Visibility**: public
- **Source Range**: 2081:189:136

## Implementation

```solidity
function getMainnetVaults() public view returns (address[] memory ans) {
    ans = new address[](1);
    ans[0] = addresses[1][Contract.CashStrategyVault];
    return ans;
}
```

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: ProposeSafeTxSetTotalAssetsCapScript.getMainnetVaults() (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
```
