# Function: transferOwnership(address)

**Contract**: [lib/aave-v3-origin/src/contracts/protocol/configuration/PoolAddressesProvider.sol/contract_PoolAddressesProvider.md]

## Metadata

- **Contract**: PoolAddressesProvider
- **Signature**: `transferOwnership(address)`
- **Visibility**: public
- **Source Range**: 1876:226:5
- **Inherited From**: Ownable

## Implementation

```solidity
///  @dev Transfers ownership of the contract to a new account (`newOwner`).
///  Can only be called by the current owner.
function transferOwnership(address newOwner) virtual public onlyOwner() {
    require(newOwner != address(0), "Ownable: new owner is the zero address");
    emit OwnershipTransferred(_owner, newOwner);
    _owner = newOwner;
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

- **_owner** (`address`)

## State Variable Writes

- **_owner** (`address`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: Ownable.transferOwnership(address) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
  └─ [1] 🔒 MODIFIER: Ownable.onlyOwner() (NodeID: 1)
      💬 Args: [no args]
    └─ [2] ⚙️ FUNCTION: Context._msgSender() (NodeID: 2)
        💬 Args: [no args]
        👁️  Def: internal
```

## Documentation

### Function Documentation

 @dev Transfers ownership of the contract to a new account (`newOwner`).
 Can only be called by the current owner.
