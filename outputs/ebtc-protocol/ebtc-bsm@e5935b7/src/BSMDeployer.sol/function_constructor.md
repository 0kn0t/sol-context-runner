# Function: constructor()

**Contract**: [src/BSMDeployer.sol/contract_BSMDeployer.md]

## Metadata

- **Contract**: BSMDeployer
- **Signature**: `constructor()`
- **Visibility**: public
- **Source Range**: 298:36:21

## Implementation

```solidity
constructor() Ownable(msg.sender) {}
```

## Related Implementations

### (address)

- **Kind**: internal
- **Source**: 1225:187:0
- **Link**: `lib/openzeppelin-contracts/contracts/access/Ownable.sol:Ownable:constructor(address)`

```solidity
///  @dev Initializes the contract setting the address provided by the deployer as the initial owner.
constructor(address initialOwner) {
    if (initialOwner == address(0)) {
        revert OwnableInvalidOwner(address(0));
    }
    _transferOwnership(initialOwner);
}
```

### _transferOwnership(address)

- **Kind**: internal
- **Source**: 2912:187:0
- **Link**: `lib/openzeppelin-contracts/contracts/access/Ownable.sol:Ownable:_transferOwnership(address)`

```solidity
///  @dev Transfers ownership of the contract to a new account (`newOwner`).
///  Internal function without access restriction.
function _transferOwnership(address newOwner) virtual internal {
    address oldOwner = _owner;
    _owner = newOwner;
    emit OwnershipTransferred(oldOwner, newOwner);
}
```

## State Variable Reads

- **_owner** (`address`)

## State Variable Writes

- **_owner** (`address`)

## Call Tree

```
┌─ [0] 🏗️ CONSTRUCTOR: BSMDeployer.constructor() (NodeID: 0)
    💬 Args: [no args]
    🏗️  Contract: BSMDeployer
  └─ [1] 🏗️ CONSTRUCTOR: Ownable.constructor(address) (NodeID: 1)
      💬 Args: [msg.sender]
      🏗️  Contract: Ownable
    └─ [2] ⚙️ FUNCTION: Ownable._transferOwnership(address) (NodeID: 2)
        💬 Args: [initialOwner]
        👁️  Def: internal
```
