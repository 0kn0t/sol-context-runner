# Function: getAddress(bytes32)

**Contract**: [lib/aave-v3-origin/src/contracts/protocol/configuration/PoolAddressesProvider.sol/contract_PoolAddressesProvider.md]

## Metadata

- **Contract**: PoolAddressesProvider
- **Signature**: `getAddress(bytes32)`
- **Visibility**: public
- **Source Range**: 1955:103:26

## Implementation

```solidity
/// @inheritdoc IPoolAddressesProvider
function getAddress(bytes32 id) override public view returns (address) {
    return _addresses[id];
}
```

## State Variable Reads

- **_addresses** (`mapping(bytes32 => address)`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: PoolAddressesProvider.getAddress(bytes32) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
```

## Documentation

### Function Documentation

@inheritdoc IPoolAddressesProvider

### Interface Documentation

 @notice Returns an address by its identifier.
 @dev The returned address might be an EOA or a contract, potentially proxied
 @dev It returns ZERO if there is no registered address with the given id
 @param id The id
 @return The address of the registered for the specified id
