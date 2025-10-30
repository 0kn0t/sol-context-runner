# Contract: AssetChainlinkAdapter

## Metadata

- **Name**: AssetChainlinkAdapter
- **Type**: Contract
- **Path**: src/AssetChainlinkAdapter.sol
- **Documentation**:  @title AssetChainlinkAdapter contract
   @notice Helps convert asset to BTC prices by combining two different oracle readings.

## Implements Interfaces

- **AggregatorV3Interface** [src/Dependencies/AggregatorV3Interface.sol/interface_AggregatorV3Interface.md]

## State Variables

### decimals

```solidity
uint8 public constant override decimals = 18
```

### version

```solidity
uint256 public constant override version = 1
```

### MAX_DECIMALS

```solidity
///  @notice Maximum number of resulting and feed decimals
uint8 public constant MAX_DECIMALS = 18
```

### ADAPTER_PRECISION

```solidity
int256 internal constant ADAPTER_PRECISION = int256(10 ** decimals)
```

### ASSET_USD_CL_FEED

```solidity
///  @notice Price feed for (ASSET / USD) pair (asset = tBTC, cbBTC etc.)
AggregatorV3Interface public immutable ASSET_USD_CL_FEED
```

**AggregatorV3Interface**: [src/Dependencies/AggregatorV3Interface.sol/interface_AggregatorV3Interface.md]

### BTC_USD_CL_FEED

```solidity
///  @notice Price feed for (BTC / USD) pair
AggregatorV3Interface public immutable BTC_USD_CL_FEED
```

**AggregatorV3Interface**: [src/Dependencies/AggregatorV3Interface.sol/interface_AggregatorV3Interface.md]

### ASSET_FEED_FRESHNESS

```solidity
/// @notice max freshness for the ASSET/USD feed
uint256 public immutable ASSET_FEED_FRESHNESS
```

### BTC_FEED_FRESHNESS

```solidity
/// @notice max freshness for the BTC/USD feed
uint256 public immutable BTC_FEED_FRESHNESS
```

### INVERTED

```solidity
/// @notice Specifies if the price of this adapter should be inverted (BTC/ASSET instead of ASSET/BTC)
bool public immutable INVERTED
```

### ASSET_USD_PRECISION

```solidity
int256 internal immutable ASSET_USD_PRECISION
```

### BTC_USD_PRECISION

```solidity
int256 internal immutable BTC_USD_PRECISION
```

## Errors

### WrongDecimals

```solidity
error WrongDecimals();
```

### OracleRound

```solidity
error OracleRound();
```

### OracleAnswer

```solidity
error OracleAnswer();
```

### OracleStale

```solidity
error OracleStale();
```

## Public/External Functions

### constructor(contract AggregatorV3Interface,uint256,contract AggregatorV3Interface,uint256,bool)

- **Signature**: `constructor(contract AggregatorV3Interface,uint256,contract AggregatorV3Interface,uint256,bool)`
- **Visibility**: public
- **Source Range**: 1686:776:20
- **Details**: [function_constructor_contract_AggregatorV3Interface_uint256_contract_AggregatorV3Interface_uint256_bool.md](./function_constructor_contract_AggregatorV3Interface_uint256_contract_AggregatorV3Interface_uint256_bool.md)

**Signature:**
```solidity
///  @notice Contract constructor
///  @param _assetUsdClFeed AggregatorV3Interface contract feed for Asset -> USD
///  @param _btcUsdClFeed AggregatorV3Interface contract feed for BTC -> USD
constructor(AggregatorV3Interface _assetUsdClFeed, uint256 _maxAssetFreshness, AggregatorV3Interface _btcUsdClFeed, uint256 _maxBtcFreshness, bool _inverted);
```

### description()

- **Signature**: `description()`
- **Visibility**: external
- **Source Range**: 2468:219:20
- **Details**: [function_description.md](./function_description.md)

**Signature:**
```solidity
function description() external view returns (string memory);
```

### getRoundData(uint80)

- **Signature**: `getRoundData(uint80)`
- **Visibility**: external
- **Source Range**: 4014:228:20
- **Details**: [function_getRoundData_uint80.md](./function_getRoundData_uint80.md)

**Signature:**
```solidity
/// @dev Needed because we inherit from AggregatorV3Interface
function getRoundData(uint80 _roundId) external view returns (uint80 roundId, int256 answer, uint256 startedAt, uint256 updatedAt, uint80 answeredInRound);
```

### latestRoundData()

- **Signature**: `latestRoundData()`
- **Visibility**: external
- **Source Range**: 4322:602:20
- **Details**: [function_latestRoundData.md](./function_latestRoundData.md)

**Signature:**
```solidity
/// @notice `roundId`, `startedAt` and `answeredInRound` are not used
function latestRoundData() external view returns (uint80 roundId, int256 answer, uint256 startedAt, uint256 updatedAt, uint80 answeredInRound);
```
