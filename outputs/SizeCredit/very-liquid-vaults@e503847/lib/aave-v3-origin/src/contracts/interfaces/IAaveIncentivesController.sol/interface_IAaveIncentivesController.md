# Interface: IAaveIncentivesController

## Metadata

- **Name**: IAaveIncentivesController
- **Type**: Interface
- **Path**: lib/aave-v3-origin/src/contracts/interfaces/IAaveIncentivesController.sol
- **Documentation**:  @title IAaveIncentivesController
   @author Aave
   @notice Defines the basic interface for an Aave Incentives Controller.
   @dev It only contains one single function, needed as a hook on aToken and debtToken transfers.

## Public/External Functions

### handleAction(address,uint256,uint256)

- **Signature**: `handleAction(address,uint256,uint256)`
- **Visibility**: external
- **Source Range**: 752:87:2

**Signature:**
```solidity
///  @dev Called by the corresponding asset on transfer hook in order to update the rewards distribution.
///  @dev The units of `totalSupply` and `userBalance` should be the same.
///  @param user The address of the user whose asset balance has changed
///  @param totalSupply The total supply of the asset prior to user balance change
///  @param userBalance The previous user balance prior to balance change
function handleAction(address user, uint256 totalSupply, uint256 userBalance) external;;
```
