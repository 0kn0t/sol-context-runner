# Function: canProcess(uint256,address)

**Contract**: [src/OraclePriceConstraint.sol/contract_OraclePriceConstraint.md]

## Metadata

- **Contract**: OraclePriceConstraint
- **Signature**: `canProcess(uint256,address)`
- **Visibility**: external
- **Source Range**: 3571:609:41

## Implementation

```solidity
/// @notice Determines if minting is allowed based on the current asset price
///  @param _amount The amount of tokens requested to mint (unused in this contract)
///  @param _bsm The address requesting to mint (unused in this contract)
///  @return bool True if minting is allowed, false otherwise
///  @return bytes Encoded error data if minting is not allowed
function canProcess(uint256 _amount, address _bsm) external view returns (bool, bytes memory) {
    uint256 assetPrice = _getAssetPrice();
    /// @dev peg price is 1e18
    uint256 minAcceptablePrice = (1e18 * minPriceBPS) / BPS;
    if (minAcceptablePrice <= assetPrice) {
        return (true, "");
    } else {
        return (false, abi.encodeWithSelector(BelowMinPrice.selector, assetPrice, minAcceptablePrice));
    }
}
```

## Related Implementations

### _getAssetPrice()

- **Kind**: internal
- **Source**: 2656:530:41
- **Link**: `src/OraclePriceConstraint.sol:OraclePriceConstraint:_getAssetPrice()`

```solidity
/// @notice Retrieves the latest price of the asset from the oracle
///  @return The latest asset price normalized to 1e18 precision
function _getAssetPrice() private view returns (uint256) {
    (, int256 answer, , uint256 updatedAt, ) = ASSET_FEED.latestRoundData();
    if (answer <= 0) revert BadOraclePrice(answer);
    /// @dev Extra staleness check here in case an actual chainlink feed
    ///  is used here instead of AssetChainlinkAdapter
    if ((block.timestamp - updatedAt) > oracleFreshnessSeconds) {
        revert StaleOraclePrice(updatedAt);
    }
    return (uint256(answer) * 1e18) / ASSET_FEED_PRECISION;
}
```

## State Variable Reads

- **minPriceBPS** (`uint256`)
- **BPS** (`uint256`)
- **ASSET_FEED** (`contract AggregatorV3Interface`) [src/Dependencies/AggregatorV3Interface.sol/interface_AggregatorV3Interface.md]
- **oracleFreshnessSeconds** (`uint256`)
- **ASSET_FEED_PRECISION** (`uint256`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: OraclePriceConstraint.canProcess(uint256,address) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: external
  └─ [1] ⚙️ FUNCTION: OraclePriceConstraint._getAssetPrice() (NodeID: 1)
      💬 Args: [no args]
      👁️  Def: private
```

## Documentation

### Function Documentation

@notice Determines if minting is allowed based on the current asset price
 @param _amount The amount of tokens requested to mint (unused in this contract)
 @param _bsm The address requesting to mint (unused in this contract)
 @return bool True if minting is allowed, false otherwise
 @return bytes Encoded error data if minting is not allowed
