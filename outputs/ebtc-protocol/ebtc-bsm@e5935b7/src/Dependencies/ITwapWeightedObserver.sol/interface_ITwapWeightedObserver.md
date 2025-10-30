# Interface: ITwapWeightedObserver

## Metadata

- **Name**: ITwapWeightedObserver
- **Type**: Interface
- **Path**: src/Dependencies/ITwapWeightedObserver.sol

## Structs

### PackedData

```solidity
struct PackedData {
    uint128 observerCumuVal;
    uint128 accumulator;
    uint64 lastObserved;
    uint64 lastAccrued;
    uint128 lastObservedAverage;
}
```

## Public/External Functions

### PERIOD()

- **Signature**: `PERIOD()`
- **Visibility**: external
- **Source Range**: 1411:50:36

**Signature:**
```solidity
function PERIOD() external view returns (uint256);;
```

### getLatestAccumulator()

- **Signature**: `getLatestAccumulator()`
- **Visibility**: external
- **Source Range**: 1467:64:36

**Signature:**
```solidity
function getLatestAccumulator() external view returns (uint128);;
```

### valueToTrack()

- **Signature**: `valueToTrack()`
- **Visibility**: external
- **Source Range**: 1536:56:36

**Signature:**
```solidity
function valueToTrack() external view returns (uint128);;
```

### getData()

- **Signature**: `getData()`
- **Visibility**: external
- **Source Range**: 1598:61:36

**Signature:**
```solidity
function getData() external view returns (PackedData memory);;
```

### observe()

- **Signature**: `observe()`
- **Visibility**: external
- **Source Range**: 1665:46:36

**Signature:**
```solidity
function observe() external returns (uint256);;
```
