# Contract: BSMDeployer

## Metadata

- **Name**: BSMDeployer
- **Type**: Contract
- **Path**: src/BSMDeployer.sol

## State Variables

### _owner (inherited from Ownable)

```solidity
address private _owner
```

## Errors

### OwnableUnauthorizedAccount (inherited from Ownable)

```solidity
///  @dev The caller account is not authorized to perform an operation.
error OwnableUnauthorizedAccount(address account);
```

### OwnableInvalidOwner (inherited from Ownable)

```solidity
///  @dev The owner is not a valid owner account. (eg. `address(0)`)
error OwnableInvalidOwner(address owner);
```

## Events

### OwnershipTransferred (inherited from Ownable)

```solidity
event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);
```

### ContractDeployed

```solidity
event ContractDeployed(address indexed bsm, address indexed escrow);
```

## Public/External Functions

### constructor()

- **Signature**: `constructor()`
- **Visibility**: public
- **Source Range**: 298:36:21
- **Details**: [function_constructor.md](./function_constructor.md)

**Signature:**
```solidity
constructor() Ownable(msg.sender);
```

### deploy(address,address,address,address,address,address,address)

- **Signature**: `deploy(address,address,address,address,address,address,address)`
- **Visibility**: external
- **Source Range**: 557:852:21
- **Details**: [function_deploy_address_address_address_address_address_address_address.md](./function_deploy_address_address_address_address_address_address_address.md)

**Signature:**
```solidity
/// @notice Deploy the BSM contract and the Escrow in a single transaction.
/// @dev Initializes the bsm with the recently deployed escrow, prevents users from calling the bsm 
/// until initialized.
function deploy(address _assetToken, address _oraclePriceConstraint, address _rateLimitingConstraint, address _buyAssetConstraint, address _ebtcToken, address _feeRecipient, address _governance) external onlyOwner();
```

### owner() (inherited from Ownable)

- **Signature**: `owner()`
- **Visibility**: public
- **Source Range**: 1638:85:0
- **Details**: [function_owner.md](./function_owner.md)

**Signature:**
```solidity
///  @dev Returns the address of the current owner.
function owner() virtual public view returns (address);
```

### renounceOwnership() (inherited from Ownable)

- **Signature**: `renounceOwnership()`
- **Visibility**: public
- **Source Range**: 2293:101:0
- **Details**: [function_renounceOwnership.md](./function_renounceOwnership.md)

**Signature:**
```solidity
///  @dev Leaves the contract without owner. It will not be possible to call
///  `onlyOwner` functions. Can only be called by the current owner.
///  NOTE: Renouncing ownership will leave the contract without an owner,
///  thereby disabling any functionality that is only available to the owner.
function renounceOwnership() virtual public onlyOwner();
```

### transferOwnership(address) (inherited from Ownable)

- **Signature**: `transferOwnership(address)`
- **Visibility**: public
- **Source Range**: 2543:215:0
- **Details**: [function_transferOwnership_address.md](./function_transferOwnership_address.md)

**Signature:**
```solidity
///  @dev Transfers ownership of the contract to a new account (`newOwner`).
///  Can only be called by the current owner.
function transferOwnership(address newOwner) virtual public onlyOwner();
```
