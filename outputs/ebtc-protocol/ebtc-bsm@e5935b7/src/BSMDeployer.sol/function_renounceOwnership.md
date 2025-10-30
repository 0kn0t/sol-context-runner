# Function: renounceOwnership()

**Contract**: [src/BSMDeployer.sol/contract_BSMDeployer.md]

## Metadata

- **Contract**: BSMDeployer
- **Signature**: `renounceOwnership()`
- **Visibility**: public
- **Source Range**: 2293:101:0
- **Inherited From**: Ownable

## Implementation

```solidity
///  @dev Leaves the contract without owner. It will not be possible to call
///  `onlyOwner` functions. Can only be called by the current owner.
///  NOTE: Renouncing ownership will leave the contract without an owner,
///  thereby disabling any functionality that is only available to the owner.
function renounceOwnership() virtual public onlyOwner() {
    _transferOwnership(address(0));
}
```

## Related Implementations

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

### onlyOwner()

- **Kind**: modifier
- **Source**: 1500:62:0
- **Link**: `lib/openzeppelin-contracts/contracts/access/Ownable.sol:Ownable:onlyOwner()`

```solidity
///  @dev Throws if called by any account other than the owner.
modifier onlyOwner() {
    _checkOwner();
    _;
}
```

### _checkOwner()

- **Kind**: internal
- **Source**: 1796:162:0
- **Link**: `lib/openzeppelin-contracts/contracts/access/Ownable.sol:Ownable:_checkOwner()`

```solidity
///  @dev Throws if the sender is not the owner.
function _checkOwner() virtual internal view {
    if (owner() != _msgSender()) {
        revert OwnableUnauthorizedAccount(_msgSender());
    }
}
```

### _msgSender()

- **Kind**: internal
- **Source**: 656:96:12
- **Link**: `lib/openzeppelin-contracts/contracts/utils/Context.sol:Context:_msgSender()`

```solidity
function _msgSender() virtual internal view returns (address) {
    return msg.sender;
}
```

### owner()

- **Kind**: internal
- **Source**: 1638:85:0
- **Link**: `lib/openzeppelin-contracts/contracts/access/Ownable.sol:Ownable:owner()`

```solidity
///  @dev Returns the address of the current owner.
function owner() virtual public view returns (address) {
    return _owner;
}
```

## State Variable Reads

- **_owner** (`address`)

## State Variable Writes

- **_owner** (`address`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: Ownable.renounceOwnership() (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
  ├─ [1] ⚙️ FUNCTION: Ownable._transferOwnership(address) (NodeID: 1)
  │   💬 Args: [address(0)]
  │   👁️  Def: internal
  └─ [1] 🔒 MODIFIER: Ownable.onlyOwner() (NodeID: 2)
      💬 Args: [no args]
    └─ [2] ⚙️ FUNCTION: Ownable._checkOwner() (NodeID: 3)
        💬 Args: [no args]
        👁️  Def: internal
      ├─ [3] ⚙️ FUNCTION: Context._msgSender() (NodeID: 4)
      │   💬 Args: [no args]
      │   👁️  Def: internal
      ├─ [3] ⚙️ FUNCTION: Ownable.owner() (NodeID: 5)
      │   💬 Args: [no args]
      │   👁️  Def: public
      └─ [3] ⚙️ FUNCTION: Context._msgSender() (NodeID: 6)
          💬 Args: [no args]
          👁️  Def: internal
```

## Documentation

### Function Documentation

 @dev Leaves the contract without owner. It will not be possible to call
 `onlyOwner` functions. Can only be called by the current owner.
 NOTE: Renouncing ownership will leave the contract without an owner,
 thereby disabling any functionality that is only available to the owner.
