# Function: description()

**Contract**: [src/AssetChainlinkAdapter.sol/contract_AssetChainlinkAdapter.md]

## Metadata

- **Contract**: AssetChainlinkAdapter
- **Signature**: `description()`
- **Visibility**: external
- **Source Range**: 2468:219:20

## Implementation

```solidity
function description() external view returns (string memory) {
    if (INVERTED) {
        return "BTC/ASSET Chainlink Adapter";
    } else {
        return "ASSET/BTC Chainlink Adapter";
    }
}
```

## State Variable Reads

- **INVERTED** (`bool`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: AssetChainlinkAdapter.description() (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: external
```
