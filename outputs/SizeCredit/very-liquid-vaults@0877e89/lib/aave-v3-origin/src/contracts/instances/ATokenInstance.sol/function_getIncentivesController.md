# Function: getIncentivesController()

**Contract**: [lib/aave-v3-origin/src/contracts/instances/ATokenInstance.sol/contract_ATokenInstance.md]

## Metadata

- **Contract**: ATokenInstance
- **Signature**: `getIncentivesController()`
- **Visibility**: external
- **Source Range**: 3625:132:35
- **Inherited From**: IncentivizedERC20

## Implementation

```solidity
///  @notice Returns the address of the Incentives Controller contract
///  @return The address of the Incentives Controller
function getIncentivesController() virtual external view returns (IAaveIncentivesController) {
    return _incentivesController;
}
```

## State Variable Reads

- **_incentivesController** (`contract IAaveIncentivesController`) [lib/aave-v3-origin/src/contracts/interfaces/IAaveIncentivesController.sol/interface_IAaveIncentivesController.md]

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: IncentivizedERC20.getIncentivesController() (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: external
```

## Documentation

### Function Documentation

 @notice Returns the address of the Incentives Controller contract
 @return The address of the Incentives Controller
