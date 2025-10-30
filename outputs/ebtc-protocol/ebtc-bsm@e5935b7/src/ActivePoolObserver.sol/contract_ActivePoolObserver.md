# Contract: ActivePoolObserver

## Metadata

- **Name**: ActivePoolObserver
- **Type**: Contract
- **Path**: src/ActivePoolObserver.sol
- **Documentation**:  @title ActivePoolObserver
   @notice Observes the average value of a pool using a TWAP (Time-Weighted Average Price) mechanism.
   @dev This contract interacts with an immutable TWAP observer to calculate virtual average values 
        and virtually sync TWAP based on elapsed time, and weights the average based on time
        passed since the last observation.

## State Variables

### OBSERVER

```solidity
/// @notice set this to the ActivePool
ITwapWeightedObserver public immutable OBSERVER
```

**ITwapWeightedObserver**: [src/Dependencies/ITwapWeightedObserver.sol/interface_ITwapWeightedObserver.md]

## Public/External Functions

### constructor(contract ITwapWeightedObserver)

- **Signature**: `constructor(contract ITwapWeightedObserver)`
- **Visibility**: public
- **Source Range**: 767:82:19
- **Details**: [function_constructor_contract_ITwapWeightedObserver.md](./function_constructor_contract_ITwapWeightedObserver.md)

**Signature:**
```solidity
///  @notice Contract constructor
///  @param _observer Address of the TWAP observer contract
constructor(ITwapWeightedObserver _observer);
```

### observe()

- **Signature**: `observe()`
- **Visibility**: external
- **Source Range**: 2993:1280:19
- **Details**: [function_observe.md](./function_observe.md)

**Signature:**
```solidity
///  @notice Observes and calculates the current average value using virtual weighting.
///  @dev Applies the new accumulator to skew the price proportionally to the time passed.
///       The virtual average is calculated using the accumulator difference and weighted based on
///       time elapsed since the last observation.
///  
///       It calculates a weighted mean of the last observed average and the virtual
///       average based on the future weight, unless no time has passed since the last observation.
///  
///  @return weightedMean The calculated weighted mean price.
function observe() external view returns (uint256);
```
