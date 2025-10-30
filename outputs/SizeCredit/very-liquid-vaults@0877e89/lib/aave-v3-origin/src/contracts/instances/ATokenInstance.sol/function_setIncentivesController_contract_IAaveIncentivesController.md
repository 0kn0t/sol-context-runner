# Function: setIncentivesController(contract IAaveIncentivesController)

**Contract**: [lib/aave-v3-origin/src/contracts/instances/ATokenInstance.sol/contract_ATokenInstance.md]

## Metadata

- **Contract**: ATokenInstance
- **Signature**: `setIncentivesController(contract IAaveIncentivesController)`
- **Visibility**: external
- **Source Range**: 3872:139:35
- **Inherited From**: IncentivizedERC20

## Implementation

```solidity
///  @notice Sets a new Incentives Controller
///  @param controller the new Incentives controller
function setIncentivesController(IAaveIncentivesController controller) external onlyPoolAdmin() {
    _incentivesController = controller;
}
```

## Related Implementations

### onlyPoolAdmin()

- **Kind**: modifier
- **Source**: 1175:194:35
- **Link**: `lib/aave-v3-origin/src/contracts/protocol/tokenization/base/IncentivizedERC20.sol:IncentivizedERC20:onlyPoolAdmin()`

```solidity
///  @dev Only pool admin can call functions marked by this modifier.
modifier onlyPoolAdmin() {
    IACLManager aclManager = IACLManager(_addressesProvider.getACLManager());
    require(aclManager.isPoolAdmin(msg.sender), Errors.CALLER_NOT_POOL_ADMIN);
    _;
}
```

## State Variable Reads

- **_addressesProvider** (`contract IPoolAddressesProvider`) [lib/aave-v3-origin/src/contracts/interfaces/IPoolAddressesProvider.sol/interface_IPoolAddressesProvider.md]

## State Variable Writes

- **_incentivesController** (`contract IAaveIncentivesController`) [lib/aave-v3-origin/src/contracts/interfaces/IAaveIncentivesController.sol/interface_IAaveIncentivesController.md]

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: IncentivizedERC20.setIncentivesController(contract IAaveIncentivesController) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: external
  └─ [1] 🔒 MODIFIER: IncentivizedERC20.onlyPoolAdmin() (NodeID: 1)
      💬 Args: [no args]
```

## Documentation

### Function Documentation

 @notice Sets a new Incentives Controller
 @param controller the new Incentives controller
