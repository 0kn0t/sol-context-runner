# Function: getACLManager()

**Contract**: [lib/aave-v3-origin/src/contracts/protocol/configuration/PoolAddressesProvider.sol/contract_PoolAddressesProvider.md]

## Metadata

- **Contract**: PoolAddressesProvider
- **Signature**: `getACLManager()`
- **Visibility**: external
- **Source Range**: 4153:107:26

## Implementation

```solidity
/// @inheritdoc IPoolAddressesProvider
function getACLManager() override external view returns (address) {
    return getAddress(ACL_MANAGER);
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

- **ACL_MANAGER** (`bytes32`)
- **_addresses** (`mapping(bytes32 => address)`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: PoolAddressesProvider.getACLManager() (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: external
  └─ [1] ⚙️ FUNCTION: PoolAddressesProvider.getAddress(bytes32) (NodeID: 1)
      💬 Args: [ACL_MANAGER]
      👁️  Def: public
```

## Documentation

### Function Documentation

@inheritdoc IPoolAddressesProvider

### Interface Documentation

 @notice Returns the address of the ACL manager.
 @return The address of the ACLManager
