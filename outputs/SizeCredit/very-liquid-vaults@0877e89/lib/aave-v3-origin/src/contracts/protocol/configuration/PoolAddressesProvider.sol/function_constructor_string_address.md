# Function: constructor(string,address)

**Contract**: [lib/aave-v3-origin/src/contracts/protocol/configuration/PoolAddressesProvider.sol/contract_PoolAddressesProvider.md]

## Metadata

- **Contract**: PoolAddressesProvider
- **Signature**: `constructor(string,address)`
- **Visibility**: public
- **Source Range**: 1497:114:26

## Implementation

```solidity
///  @dev Constructor.
///  @param marketId The identifier of the market.
///  @param owner The owner address of this contract.
constructor(string memory marketId, address owner) {
    _setMarketId(marketId);
    transferOwnership(owner);
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

### transferOwnership(address)

- **Kind**: internal
- **Source**: 1876:226:5
- **Link**: `lib/aave-v3-origin/src/contracts/dependencies/openzeppelin/contracts/Ownable.sol:Ownable:transferOwnership(address)`

```solidity
///  @dev Transfers ownership of the contract to a new account (`newOwner`).
///  Can only be called by the current owner.
function transferOwnership(address newOwner) virtual public onlyOwner() {
    require(newOwner != address(0), "Ownable: new owner is the zero address");
    emit OwnershipTransferred(_owner, newOwner);
    _owner = newOwner;
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
- **_owner** (`address`)

## Call Tree

```
┌─ [0] 🏗️ CONSTRUCTOR: PoolAddressesProvider.constructor(string,address) (NodeID: 0)
    💬 Args: [no args]
    🏗️  Contract: PoolAddressesProvider
  ├─ [1] ⚙️ FUNCTION: PoolAddressesProvider._setMarketId(string) (NodeID: 1)
  │   💬 Args: [marketId]
  │   👁️  Def: internal
  └─ [1] ⚙️ FUNCTION: Ownable.transferOwnership(address) (NodeID: 2)
      💬 Args: [owner]
      👁️  Def: public
    └─ [2] 🔒 MODIFIER: Ownable.onlyOwner() (NodeID: 3)
        💬 Args: [no args]
      └─ [3] ⚙️ FUNCTION: Context._msgSender() (NodeID: 4)
          💬 Args: [no args]
          👁️  Def: internal
```

## Documentation

### Function Documentation

 @dev Constructor.
 @param marketId The identifier of the market.
 @param owner The owner address of this contract.
