# Function: setMarketId(string)

**Contract**: [lib/aave-v3-origin/src/contracts/protocol/configuration/PoolAddressesProvider.sol/contract_PoolAddressesProvider.md]

## Metadata

- **Contract**: PoolAddressesProvider
- **Signature**: `setMarketId(string)`
- **Visibility**: external
- **Source Range**: 1798:112:26

## Implementation

```solidity
/// @inheritdoc IPoolAddressesProvider
function setMarketId(string memory newMarketId) override external onlyOwner() {
    _setMarketId(newMarketId);
}
```

## Related Implementations

### _setMarketId(string)

- **Kind**: internal
- **Source**: 7355:183:26
- **Link**: `lib/aave-v3-origin/src/contracts/protocol/configuration/PoolAddressesProvider.sol:PoolAddressesProvider:_setMarketId(string)`

```solidity
///  @notice Updates the identifier of the Aave market.
///  @param newMarketId The new id of the market
function _setMarketId(string memory newMarketId) internal {
    string memory oldMarketId = _marketId;
    _marketId = newMarketId;
    emit MarketIdSet(oldMarketId, newMarketId);
}
```

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

- **_marketId** (`string`)
- **_owner** (`address`)

## State Variable Writes

- **_marketId** (`string`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: PoolAddressesProvider.setMarketId(string) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: external
  ├─ [1] ⚙️ FUNCTION: PoolAddressesProvider._setMarketId(string) (NodeID: 1)
  │   💬 Args: [newMarketId]
  │   👁️  Def: internal
  └─ [1] 🔒 MODIFIER: Ownable.onlyOwner() (NodeID: 2)
      💬 Args: [no args]
    └─ [2] ⚙️ FUNCTION: Context._msgSender() (NodeID: 3)
        💬 Args: [no args]
        👁️  Def: internal
```

## Documentation

### Function Documentation

@inheritdoc IPoolAddressesProvider

### Interface Documentation

 @notice Associates an id with a specific PoolAddressesProvider.
 @dev This can be used to create an onchain registry of PoolAddressesProviders to
 identify and validate multiple Aave markets.
 @param newMarketId The market id
