# Function: getPriceOracleSentinel()

**Contract**: [lib/aave-v3-origin/src/contracts/protocol/configuration/PoolAddressesProvider.sol/contract_PoolAddressesProvider.md]

## Metadata

- **Contract**: PoolAddressesProvider
- **Signature**: `getPriceOracleSentinel()`
- **Visibility**: external
- **Source Range**: 4995:126:26

## Implementation

```solidity
/// @inheritdoc IPoolAddressesProvider
function getPriceOracleSentinel() override external view returns (address) {
    return getAddress(PRICE_ORACLE_SENTINEL);
}
```

## Related Implementations

### getAddress(bytes32)

- **Kind**: internal
- **Source**: 1955:103:26
- **Link**: `lib/aave-v3-origin/src/contracts/protocol/configuration/PoolAddressesProvider.sol:PoolAddressesProvider:getAddress(bytes32)`

```solidity
/// @inheritdoc IPoolAddressesProvider
function getAddress(bytes32 id) override public view returns (address) {
    return _addresses[id];
}
```

## State Variable Reads

- **PRICE_ORACLE_SENTINEL** (`bytes32`)
- **_addresses** (`mapping(bytes32 => address)`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: PoolAddressesProvider.getPriceOracleSentinel() (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: external
  └─ [1] ⚙️ FUNCTION: PoolAddressesProvider.getAddress(bytes32) (NodeID: 1)
      💬 Args: [PRICE_ORACLE_SENTINEL]
      👁️  Def: public
```

## Documentation

### Function Documentation

@inheritdoc IPoolAddressesProvider

### Interface Documentation

 @notice Returns the address of the price oracle sentinel.
 @return The address of the PriceOracleSentinel
