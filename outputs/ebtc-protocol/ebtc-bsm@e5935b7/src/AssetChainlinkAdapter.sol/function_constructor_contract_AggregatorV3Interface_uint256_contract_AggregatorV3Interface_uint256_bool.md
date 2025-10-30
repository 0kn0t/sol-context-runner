# Function: constructor(contract AggregatorV3Interface,uint256,contract AggregatorV3Interface,uint256,bool)

**Contract**: [src/AssetChainlinkAdapter.sol/contract_AssetChainlinkAdapter.md]

## Metadata

- **Contract**: AssetChainlinkAdapter
- **Signature**: `constructor(contract AggregatorV3Interface,uint256,contract AggregatorV3Interface,uint256,bool)`
- **Visibility**: public
- **Source Range**: 1686:776:20

## Implementation

```solidity
///  @notice Contract constructor
///  @param _assetUsdClFeed AggregatorV3Interface contract feed for Asset -> USD
///  @param _btcUsdClFeed AggregatorV3Interface contract feed for BTC -> USD
constructor(AggregatorV3Interface _assetUsdClFeed, uint256 _maxAssetFreshness, AggregatorV3Interface _btcUsdClFeed, uint256 _maxBtcFreshness, bool _inverted) {
    ASSET_USD_CL_FEED = AggregatorV3Interface(_assetUsdClFeed);
    ASSET_FEED_FRESHNESS = _maxAssetFreshness;
    BTC_USD_CL_FEED = AggregatorV3Interface(_btcUsdClFeed);
    BTC_FEED_FRESHNESS = _maxBtcFreshness;
    INVERTED = _inverted;
    require(ASSET_USD_CL_FEED.decimals() <= MAX_DECIMALS, WrongDecimals());
    require(BTC_USD_CL_FEED.decimals() <= MAX_DECIMALS, WrongDecimals());
    ASSET_USD_PRECISION = int256(10 ** ASSET_USD_CL_FEED.decimals());
    BTC_USD_PRECISION = int256(10 ** BTC_USD_CL_FEED.decimals());
}
```

## External Calls

- **AggregatorV3Interface::decimals()**

## State Variable Reads

- **ASSET_USD_CL_FEED** (`contract AggregatorV3Interface`) [src/Dependencies/AggregatorV3Interface.sol/interface_AggregatorV3Interface.md]
- **MAX_DECIMALS** (`uint8`)
- **BTC_USD_CL_FEED** (`contract AggregatorV3Interface`) [src/Dependencies/AggregatorV3Interface.sol/interface_AggregatorV3Interface.md]

## State Variable Writes

- **ASSET_USD_CL_FEED** (`contract AggregatorV3Interface`) [src/Dependencies/AggregatorV3Interface.sol/interface_AggregatorV3Interface.md]
- **ASSET_FEED_FRESHNESS** (`uint256`)
- **BTC_USD_CL_FEED** (`contract AggregatorV3Interface`) [src/Dependencies/AggregatorV3Interface.sol/interface_AggregatorV3Interface.md]
- **BTC_FEED_FRESHNESS** (`uint256`)
- **INVERTED** (`bool`)
- **ASSET_USD_PRECISION** (`int256`)
- **BTC_USD_PRECISION** (`int256`)

## Call Tree

```
┌─ [0] 🏗️ CONSTRUCTOR: AssetChainlinkAdapter.constructor(contract AggregatorV3Interface,uint256,contract AggregatorV3Interface,uint256,bool) (NodeID: 0)
    💬 Args: [no args]
    🏗️  Contract: AssetChainlinkAdapter
```

## Documentation

### Function Documentation

 @notice Contract constructor
 @param _assetUsdClFeed AggregatorV3Interface contract feed for Asset -> USD
 @param _btcUsdClFeed AggregatorV3Interface contract feed for BTC -> USD
