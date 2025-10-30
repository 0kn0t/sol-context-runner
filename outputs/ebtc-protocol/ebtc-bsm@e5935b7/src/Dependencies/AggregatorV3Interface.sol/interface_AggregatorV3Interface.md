# Interface: AggregatorV3Interface

## Metadata

- **Name**: AggregatorV3Interface
- **Type**: Interface
- **Path**: src/Dependencies/AggregatorV3Interface.sol

## Public/External Functions

### decimals()

- **Signature**: `decimals()`
- **Visibility**: external
- **Source Range**: 156:50:23

**Signature:**
```solidity
function decimals() external view returns (uint8);;
```

### description()

- **Signature**: `description()`
- **Visibility**: external
- **Source Range**: 210:61:23

**Signature:**
```solidity
function description() external view returns (string memory);;
```

### version()

- **Signature**: `version()`
- **Visibility**: external
- **Source Range**: 275:51:23

**Signature:**
```solidity
function version() external view returns (uint256);;
```

### getRoundData(uint80)

- **Signature**: `getRoundData(uint80)`
- **Visibility**: external
- **Source Range**: 330:163:23

**Signature:**
```solidity
function getRoundData(uint80 _roundId) external view returns (uint80 roundId, int256 answer, uint256 startedAt, uint256 updatedAt, uint80 answeredInRound);;
```

### latestRoundData()

- **Signature**: `latestRoundData()`
- **Visibility**: external
- **Source Range**: 497:155:23

**Signature:**
```solidity
function latestRoundData() external view returns (uint80 roundId, int256 answer, uint256 startedAt, uint256 updatedAt, uint80 answeredInRound);;
```
