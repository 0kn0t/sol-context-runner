# Function: deploy(address,address,address,address,address,address,address)

**Contract**: [src/BSMDeployer.sol/contract_BSMDeployer.md]

## Metadata

- **Contract**: BSMDeployer
- **Signature**: `deploy(address,address,address,address,address,address,address)`
- **Visibility**: external
- **Source Range**: 557:852:21

## Implementation

```solidity
/// @notice Deploy the BSM contract and the Escrow in a single transaction.
/// @dev Initializes the bsm with the recently deployed escrow, prevents users from calling the bsm 
/// until initialized.
function deploy(address _assetToken, address _oraclePriceConstraint, address _rateLimitingConstraint, address _buyAssetConstraint, address _ebtcToken, address _feeRecipient, address _governance) external onlyOwner() {
    EbtcBSM bsm = new EbtcBSM(address(_assetToken), address(_oraclePriceConstraint), address(_rateLimitingConstraint), address(_buyAssetConstraint), address(_ebtcToken), address(_governance));
    BaseEscrow escrow = new BaseEscrow(address(_assetToken), address(bsm), address(_governance), address(_feeRecipient));
    bsm.initialize(address(escrow));
    emit ContractDeployed(address(bsm), address(escrow));
}
```

## Related Implementations

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

## External Calls

- **EbtcBSM::initialize(address)**

## State Variable Reads

- **_owner** (`address`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: BSMDeployer.deploy(address,address,address,address,address,address,address) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: external
  └─ [1] 🔒 MODIFIER: Ownable.onlyOwner() (NodeID: 1)
      💬 Args: [no args]
    └─ [2] ⚙️ FUNCTION: Ownable._checkOwner() (NodeID: 2)
        💬 Args: [no args]
        👁️  Def: internal
      ├─ [3] ⚙️ FUNCTION: Context._msgSender() (NodeID: 3)
      │   💬 Args: [no args]
      │   👁️  Def: internal
      ├─ [3] ⚙️ FUNCTION: Ownable.owner() (NodeID: 4)
      │   💬 Args: [no args]
      │   👁️  Def: public
      └─ [3] ⚙️ FUNCTION: Context._msgSender() (NodeID: 5)
          💬 Args: [no args]
          👁️  Def: internal
```

## Documentation

### Function Documentation

@notice Deploy the BSM contract and the Escrow in a single transaction.
@dev Initializes the bsm with the recently deployed escrow, prevents users from calling the bsm 
until initialized.
