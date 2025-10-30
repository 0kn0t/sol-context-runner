# Function: onMigrateTarget(uint256)

**Contract**: [src/ERC4626Escrow.sol/contract_ERC4626Escrow.md]

## Metadata

- **Contract**: ERC4626Escrow
- **Signature**: `onMigrateTarget(uint256)`
- **Visibility**: external
- **Source Range**: 5710:106:22
- **Inherited From**: BaseEscrow

## Implementation

```solidity
/// @notice Called on the target escrow during a migration by the BSM to set the user deposit amount
///  @dev Can only be called by the bsm contract
function onMigrateTarget(uint256 _amount) external onlyBSM() {
    totalAssetsDeposited = _amount;
}
```

## Related Implementations

### onlyBSM()

- **Kind**: modifier
- **Source**: 787:115:22
- **Link**: `src/BaseEscrow.sol:BaseEscrow:onlyBSM()`

```solidity
/// @notice Modifier to restrict function calls to the BSM
modifier onlyBSM() {
    if (msg.sender != BSM) {
        revert CallerNotBSM();
    }
    _;
}
```

## State Variable Reads

- **BSM** (`address`)

## State Variable Writes

- **totalAssetsDeposited** (`uint256`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: BaseEscrow.onMigrateTarget(uint256) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: external
  └─ [1] 🔒 MODIFIER: BaseEscrow.onlyBSM() (NodeID: 1)
      💬 Args: [no args]
```

## Documentation

### Function Documentation

@notice Called on the target escrow during a migration by the BSM to set the user deposit amount
 @dev Can only be called by the bsm contract
