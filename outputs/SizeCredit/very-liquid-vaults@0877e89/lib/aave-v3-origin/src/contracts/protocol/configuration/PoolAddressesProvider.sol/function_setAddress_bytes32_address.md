# Function: setAddress(bytes32,address)

**Contract**: [lib/aave-v3-origin/src/contracts/protocol/configuration/PoolAddressesProvider.sol/contract_PoolAddressesProvider.md]

## Metadata

- **Contract**: PoolAddressesProvider
- **Signature**: `setAddress(bytes32,address)`
- **Visibility**: external
- **Source Range**: 2103:208:26

## Implementation

```solidity
/// @inheritdoc IPoolAddressesProvider
function setAddress(bytes32 id, address newAddress) override external onlyOwner() {
    address oldAddress = _addresses[id];
    _addresses[id] = newAddress;
    emit AddressSet(id, oldAddress, newAddress);
}
```

## Related Implementations

### onlyOwner()

- **Kind**: modifier
- **Source**: 1170:106:5
- **Link**: `lib/aave-v3-origin/src/contracts/dependencies/openzeppelin/contracts/Ownable.sol:Ownable:onlyOwner()`

```solidity
///  @dev Throws if called by any account other than the owner.
modifier onlyOwner() {
    require(_owner == _msgSender(), "Ownable: caller is not the owner");
    _;
}
```

### _msgSender()

- **Kind**: internal
- **Source**: 588:107:2
- **Link**: `lib/aave-v3-origin/src/contracts/dependencies/openzeppelin/contracts/Context.sol:Context:_msgSender()`

```solidity
function _msgSender() virtual internal view returns (address payable) {
    return payable(msg.sender);
}
```

## State Variable Reads

- **_addresses** (`mapping(bytes32 => address)`)
- **_owner** (`address`)

## State Variable Writes

- **_addresses** (`mapping(bytes32 => address)`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: PoolAddressesProvider.setAddress(bytes32,address) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: external
  └─ [1] 🔒 MODIFIER: Ownable.onlyOwner() (NodeID: 1)
      💬 Args: [no args]
    └─ [2] ⚙️ FUNCTION: Context._msgSender() (NodeID: 2)
        💬 Args: [no args]
        👁️  Def: internal
```

## Documentation

### Function Documentation

@inheritdoc IPoolAddressesProvider

### Interface Documentation

 @notice Sets an address for an id replacing the address saved in the addresses map.
 @dev IMPORTANT Use this function carefully, as it will do a hard replacement
 @param id The id
 @param newAddress The address to set
