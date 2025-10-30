# Function: constructor(contract ITwapWeightedObserver)

**Contract**: [src/ActivePoolObserver.sol/contract_ActivePoolObserver.md]

## Metadata

- **Contract**: ActivePoolObserver
- **Signature**: `constructor(contract ITwapWeightedObserver)`
- **Visibility**: public
- **Source Range**: 767:82:19

## Implementation

```solidity
///  @notice Contract constructor
///  @param _observer Address of the TWAP observer contract
constructor(ITwapWeightedObserver _observer) {
    OBSERVER = _observer;
}
```

## State Variable Writes

- **OBSERVER** (`contract ITwapWeightedObserver`) [src/Dependencies/ITwapWeightedObserver.sol/interface_ITwapWeightedObserver.md]

## Call Tree

```
┌─ [0] 🏗️ CONSTRUCTOR: ActivePoolObserver.constructor(contract ITwapWeightedObserver) (NodeID: 0)
    💬 Args: [no args]
    🏗️  Contract: ActivePoolObserver
```

## Documentation

### Function Documentation

 @notice Contract constructor
 @param _observer Address of the TWAP observer contract
