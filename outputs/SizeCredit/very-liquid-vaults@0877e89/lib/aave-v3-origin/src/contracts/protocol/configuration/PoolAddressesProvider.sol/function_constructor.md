# Function: constructor()

**Contract**: [lib/aave-v3-origin/src/contracts/protocol/configuration/PoolAddressesProvider.sol/contract_PoolAddressesProvider.md]

## Metadata

- **Contract**: PoolAddressesProvider
- **Signature**: `constructor()`
- **Visibility**: public
- **Source Range**: 816:135:5
- **Inherited From**: Ownable

## Implementation

```solidity
///  @dev Initializes the contract setting the deployer as the initial owner.
constructor() {
    address msgSender = _msgSender();
    _owner = msgSender;
    emit OwnershipTransferred(address(0), msgSender);
}
```

## Related Implementations

### _msgSender()

- **Kind**: internal
- **Source**: 588:107:2
- **Link**: `lib/aave-v3-origin/src/contracts/dependencies/openzeppelin/contracts/Context.sol:Context:_msgSender()`

```solidity
function _msgSender() virtual internal view returns (address payable) {
    return payable(msg.sender);
}
```

## State Variable Writes

- **_owner** (`address`)

## Call Tree

```
┌─ [0] 🏗️ CONSTRUCTOR: Ownable.constructor() (NodeID: 0)
    💬 Args: [no args]
    🏗️  Contract: Ownable
  └─ [1] ⚙️ FUNCTION: Context._msgSender() (NodeID: 1)
      💬 Args: [no args]
      👁️  Def: internal
```

## Documentation

### Function Documentation

 @dev Initializes the contract setting the deployer as the initial owner.
