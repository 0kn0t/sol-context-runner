# Function: isWhitelistedDex(address)

**Contract**: [src/BorrowerOperationsV6.sol/contract_BorrowerOperationsV6.md]

## Metadata

- **Contract**: BorrowerOperationsV6
- **Signature**: `isWhitelistedDex(address)`
- **Visibility**: external
- **Source Range**: 3058:113:12

## Implementation

```solidity
function isWhitelistedDex(address dex) external view returns (bool) {
    return whitelistedDexes[dex];
}
```

## State Variable Reads

- **whitelistedDexes** (`mapping(address => bool)`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: BorrowerOperationsV6.isWhitelistedDex(address) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: external
```
