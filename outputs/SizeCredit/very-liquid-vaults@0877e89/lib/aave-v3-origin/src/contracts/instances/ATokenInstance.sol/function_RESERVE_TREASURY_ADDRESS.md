# Function: RESERVE_TREASURY_ADDRESS()

**Contract**: [lib/aave-v3-origin/src/contracts/instances/ATokenInstance.sol/contract_ATokenInstance.md]

## Metadata

- **Contract**: ATokenInstance
- **Signature**: `RESERVE_TREASURY_ADDRESS()`
- **Visibility**: external
- **Source Range**: 3859:104:31
- **Inherited From**: AToken

## Implementation

```solidity
/// @inheritdoc IAToken
function RESERVE_TREASURY_ADDRESS() override external view returns (address) {
    return _treasury;
}
```

## State Variable Reads

- **_treasury** (`address`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: AToken.RESERVE_TREASURY_ADDRESS() (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: external
```

## Documentation

### Function Documentation

@inheritdoc IAToken

### Interface Documentation

 @notice Returns the address of the Aave treasury, receiving the fees on this aToken.
 @return Address of the Aave treasury
