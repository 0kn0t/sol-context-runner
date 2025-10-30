# Function: handleAction(address,uint256,uint256)

**Contract**: [lib/aave-v3-origin/src/contracts/mocks/helpers/MockIncentivesController.sol/contract_MockIncentivesController.md]

## Metadata

- **Contract**: MockIncentivesController
- **Signature**: `handleAction(address,uint256,uint256)`
- **Visibility**: external
- **Source Range**: 216:69:25

## Implementation

```solidity
function handleAction(address, uint256, uint256) override external {}
```

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: MockIncentivesController.handleAction(address,uint256,uint256) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: external
```

## Documentation

### Interface Documentation

 @dev Called by the corresponding asset on transfer hook in order to update the rewards distribution.
 @dev The units of `totalSupply` and `userBalance` should be the same.
 @param user The address of the user whose asset balance has changed
 @param totalSupply The total supply of the asset prior to user balance change
 @param userBalance The previous user balance prior to balance change
