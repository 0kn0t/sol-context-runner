# Function: getMarketId()

**Contract**: [lib/aave-v3-origin/src/contracts/protocol/configuration/PoolAddressesProvider.sol/contract_PoolAddressesProvider.md]

## Metadata

- **Contract**: PoolAddressesProvider
- **Signature**: `getMarketId()`
- **Visibility**: external
- **Source Range**: 1656:97:26

## Implementation

```solidity
/// @inheritdoc IPoolAddressesProvider
function getMarketId() override external view returns (string memory) {
    return _marketId;
}
```

## State Variable Reads

- **_marketId** (`string`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: PoolAddressesProvider.getMarketId() (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: external
```

## Documentation

### Function Documentation

@inheritdoc IPoolAddressesProvider

### Interface Documentation

 @notice Returns the id of the Aave market to which this contract points to.
 @return The market id
