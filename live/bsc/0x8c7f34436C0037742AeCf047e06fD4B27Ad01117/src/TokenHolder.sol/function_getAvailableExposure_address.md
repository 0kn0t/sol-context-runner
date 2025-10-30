# Function: getAvailableExposure(address)

**Contract**: [src/TokenHolder.sol/contract_TokenHolder.md]

## Metadata

- **Contract**: TokenHolder
- **Signature**: `getAvailableExposure(address)`
- **Visibility**: public
- **Source Range**: 11574:355:13

## Implementation

```solidity
function getAvailableExposure(address collateralAddress) public view returns (uint256) {
    Collateral memory collateral = collateralMapping[collateralAddress];
    if (!collateral.active) return 0;
    return (collateral.maxExposure > collateral.currentExposure) ? (collateral.maxExposure - collateral.currentExposure) : 0;
}
```

## State Variable Reads

- **collateralMapping** (`mapping(address => struct Collateral)`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: TokenHolder.getAvailableExposure(address) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
```
