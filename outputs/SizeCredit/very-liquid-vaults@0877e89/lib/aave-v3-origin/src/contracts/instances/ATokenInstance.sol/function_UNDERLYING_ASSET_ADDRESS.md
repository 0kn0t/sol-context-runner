# Function: UNDERLYING_ASSET_ADDRESS()

**Contract**: [lib/aave-v3-origin/src/contracts/instances/ATokenInstance.sol/contract_ATokenInstance.md]

## Metadata

- **Contract**: ATokenInstance
- **Signature**: `UNDERLYING_ASSET_ADDRESS()`
- **Visibility**: external
- **Source Range**: 3993:111:31
- **Inherited From**: AToken

## Implementation

```solidity
/// @inheritdoc IAToken
function UNDERLYING_ASSET_ADDRESS() override external view returns (address) {
    return _underlyingAsset;
}
```

## State Variable Reads

- **_underlyingAsset** (`address`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: AToken.UNDERLYING_ASSET_ADDRESS() (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: external
```

## Documentation

### Function Documentation

@inheritdoc IAToken

### Interface Documentation

 @notice Returns the address of the underlying asset of this aToken (E.g. WETH for aWETH)
 @return The address of the underlying asset
