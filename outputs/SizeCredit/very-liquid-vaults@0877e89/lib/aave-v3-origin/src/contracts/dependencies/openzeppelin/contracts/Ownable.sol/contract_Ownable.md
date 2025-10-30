# Contract: Ownable

## Metadata

- **Name**: Ownable
- **Type**: Contract
- **Path**: lib/aave-v3-origin/src/contracts/dependencies/openzeppelin/contracts/Ownable.sol
- **Documentation**:  @dev Contract module which provides a basic access control mechanism, where
   there is an account (an owner) that can be granted exclusive access to
   specific functions.
   By default, the owner account will be the one that deploys the contract. This
   can later be changed with {transferOwnership}.
   This module is used through inheritance. It will make available the modifier
   `onlyOwner`, which can be applied to your functions to restrict their use to
   the owner.

## State Variables

### _owner

```solidity
address private _owner
```

## Events

### OwnershipTransferred

```solidity
event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);
```

## Public/External Functions

### constructor()

- **Signature**: `constructor()`
- **Visibility**: public
- **Source Range**: 816:135:5
- **Details**: [function_constructor.md](./function_constructor.md)

**Signature:**
```solidity
///  @dev Initializes the contract setting the deployer as the initial owner.
constructor();
```

### owner()

- **Signature**: `owner()`
- **Visibility**: public
- **Source Range**: 1019:71:5
- **Details**: [function_owner.md](./function_owner.md)

**Signature:**
```solidity
///  @dev Returns the address of the current owner.
function owner() public view returns (address);
```

### renounceOwnership()

- **Signature**: `renounceOwnership()`
- **Visibility**: public
- **Source Range**: 1602:135:5
- **Details**: [function_renounceOwnership.md](./function_renounceOwnership.md)

**Signature:**
```solidity
///  @dev Leaves the contract without owner. It will not be possible to call
///  `onlyOwner` functions anymore. Can only be called by the current owner.
///  NOTE: Renouncing ownership will leave the contract without an owner,
///  thereby removing any functionality that is only available to the owner.
function renounceOwnership() virtual public onlyOwner();
```

### transferOwnership(address)

- **Signature**: `transferOwnership(address)`
- **Visibility**: public
- **Source Range**: 1876:226:5
- **Details**: [function_transferOwnership_address.md](./function_transferOwnership_address.md)

**Signature:**
```solidity
///  @dev Transfers ownership of the contract to a new account (`newOwner`).
///  Can only be called by the current owner.
function transferOwnership(address newOwner) virtual public onlyOwner();
```
