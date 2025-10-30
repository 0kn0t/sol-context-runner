# Function: getRoundData(uint80)

**Contract**: [src/AssetChainlinkAdapter.sol/contract_AssetChainlinkAdapter.md]

## Metadata

- **Contract**: AssetChainlinkAdapter
- **Signature**: `getRoundData(uint80)`
- **Visibility**: external
- **Source Range**: 4014:228:20

## Implementation

```solidity
/// @dev Needed because we inherit from AggregatorV3Interface
function getRoundData(uint80 _roundId) external view returns (uint80 roundId, int256 answer, uint256 startedAt, uint256 updatedAt, uint80 answeredInRound) {}
```

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: AssetChainlinkAdapter.getRoundData(uint80) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: external
```

## Documentation

### Function Documentation

@dev Needed because we inherit from AggregatorV3Interface
