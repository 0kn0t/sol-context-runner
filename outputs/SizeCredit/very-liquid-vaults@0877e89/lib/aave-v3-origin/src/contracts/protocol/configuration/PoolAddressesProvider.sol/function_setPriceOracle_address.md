# Function: setPriceOracle(address)

**Contract**: [lib/aave-v3-origin/src/contracts/protocol/configuration/PoolAddressesProvider.sol/contract_PoolAddressesProvider.md]

## Metadata

- **Contract**: PoolAddressesProvider
- **Signature**: `setPriceOracle(address)`
- **Visibility**: external
- **Source Range**: 3864:244:26

## Implementation

```solidity
/// @inheritdoc IPoolAddressesProvider
function setPriceOracle(address newPriceOracle) override external onlyOwner() {
    address oldPriceOracle = _addresses[PRICE_ORACLE];
    _addresses[PRICE_ORACLE] = newPriceOracle;
    emit PriceOracleUpdated(oldPriceOracle, newPriceOracle);
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
- **PRICE_ORACLE** (`bytes32`)
- **_owner** (`address`)

## State Variable Writes

- **_addresses** (`mapping(bytes32 => address)`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: PoolAddressesProvider.setPriceOracle(address) (NodeID: 0)
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

 @notice Updates the address of the price oracle.
 @param newPriceOracle The address of the new PriceOracle
