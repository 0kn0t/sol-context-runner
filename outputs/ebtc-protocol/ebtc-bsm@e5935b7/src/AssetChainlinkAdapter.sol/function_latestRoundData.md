# Function: latestRoundData()

**Contract**: [src/AssetChainlinkAdapter.sol/contract_AssetChainlinkAdapter.md]

## Metadata

- **Contract**: AssetChainlinkAdapter
- **Signature**: `latestRoundData()`
- **Visibility**: external
- **Source Range**: 4322:602:20

## Implementation

```solidity
/// @notice `roundId`, `startedAt` and `answeredInRound` are not used
function latestRoundData() external view returns (uint80 roundId, int256 answer, uint256 startedAt, uint256 updatedAt, uint80 answeredInRound) {
    (int256 assetUsdPrice, uint256 assetUsdUpdatedAt) = _latestRoundData(ASSET_USD_CL_FEED, ASSET_FEED_FRESHNESS);
    (int256 btcUsdPrice, uint256 btcUsdUpdatedAt) = _latestRoundData(BTC_USD_CL_FEED, BTC_FEED_FRESHNESS);
    updatedAt = _min(assetUsdUpdatedAt, btcUsdUpdatedAt);
    answer = _convertAnswer(btcUsdPrice, assetUsdPrice);
}
```

## Related Implementations

### _latestRoundData(contract AggregatorV3Interface,uint256)

- **Kind**: internal
- **Source**: 3507:435:20
- **Link**: `src/AssetChainlinkAdapter.sol:AssetChainlinkAdapter:_latestRoundData(contract AggregatorV3Interface,uint256)`

```solidity
function _latestRoundData(AggregatorV3Interface _feed, uint256 maxFreshness) private view returns (int256 answer, uint256 updatedAt) {
    uint80 feedRoundId;
    (feedRoundId, answer, , updatedAt, ) = _feed.latestRoundData();
    require(feedRoundId > 0, OracleRound());
    require(answer > 0, OracleAnswer());
    require((block.timestamp - updatedAt) <= maxFreshness, OracleStale());
}
```

### _min(uint256,uint256)

- **Kind**: internal
- **Source**: 2849:110:20
- **Link**: `src/AssetChainlinkAdapter.sol:AssetChainlinkAdapter:_min(uint256,uint256)`

```solidity
/// @notice returns the smallest uint256 out of the 2 parameters
///  @param _a first number to compare
///  @param _b second number to compare
function _min(uint256 _a, uint256 _b) private pure returns (uint256) {
    return (_a < _b) ? _a : _b;
}
```

### _convertAnswer(int256,int256)

- **Kind**: internal
- **Source**: 3053:448:20
- **Link**: `src/AssetChainlinkAdapter.sol:AssetChainlinkAdapter:_convertAnswer(int256,int256)`

```solidity
/// @dev Uses the prices from the asset feed and the BTC feed to compute ASSET->BTC
function _convertAnswer(int256 btcUsdPrice, int256 assetUsdPrice) private view returns (int256) {
    if (INVERTED) {
        return ((btcUsdPrice * ASSET_USD_PRECISION) * ADAPTER_PRECISION) / (BTC_USD_PRECISION * assetUsdPrice);
    } else {
        return ((assetUsdPrice * BTC_USD_PRECISION) * ADAPTER_PRECISION) / (ASSET_USD_PRECISION * btcUsdPrice);
    }
}
```

## State Variable Reads

- **ASSET_USD_CL_FEED** (`contract AggregatorV3Interface`) [src/Dependencies/AggregatorV3Interface.sol/interface_AggregatorV3Interface.md]
- **ASSET_FEED_FRESHNESS** (`uint256`)
- **BTC_USD_CL_FEED** (`contract AggregatorV3Interface`) [src/Dependencies/AggregatorV3Interface.sol/interface_AggregatorV3Interface.md]
- **BTC_FEED_FRESHNESS** (`uint256`)
- **INVERTED** (`bool`)
- **ASSET_USD_PRECISION** (`int256`)
- **ADAPTER_PRECISION** (`int256`)
- **BTC_USD_PRECISION** (`int256`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: AssetChainlinkAdapter.latestRoundData() (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: external
  ├─ [1] ⚙️ FUNCTION: AssetChainlinkAdapter._latestRoundData(contract AggregatorV3Interface,uint256) (NodeID: 1)
  │   💬 Args: [ASSET_USD_CL_FEED, ASSET_FEED_FRESHNESS]
  │   👁️  Def: private
  ├─ [1] ⚙️ FUNCTION: AssetChainlinkAdapter._latestRoundData(contract AggregatorV3Interface,uint256) (NodeID: 2)
  │   💬 Args: [BTC_USD_CL_FEED, BTC_FEED_FRESHNESS]
  │   👁️  Def: private
  ├─ [1] ⚙️ FUNCTION: AssetChainlinkAdapter._min(uint256,uint256) (NodeID: 3)
  │   💬 Args: [assetUsdUpdatedAt, btcUsdUpdatedAt]
  │   👁️  Def: private
  └─ [1] ⚙️ FUNCTION: AssetChainlinkAdapter._convertAnswer(int256,int256) (NodeID: 4)
      💬 Args: [btcUsdPrice, assetUsdPrice]
      👁️  Def: private
```

## Documentation

### Function Documentation

@notice `roundId`, `startedAt` and `answeredInRound` are not used
