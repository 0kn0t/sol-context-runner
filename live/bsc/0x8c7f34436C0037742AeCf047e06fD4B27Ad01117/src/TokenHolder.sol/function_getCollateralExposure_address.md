# Function: getCollateralExposure(address)

**Contract**: [src/TokenHolder.sol/contract_TokenHolder.md]

## Metadata

- **Contract**: TokenHolder
- **Signature**: `getCollateralExposure(address)`
- **Visibility**: public
- **Source Range**: 11345:164:13

## Implementation

```solidity
function getCollateralExposure(address collateralAddress) public view returns (uint256) {
    return collateralMapping[collateralAddress].currentExposure;
}
```

## State Variable Reads

- **collateralMapping** (`mapping(address => struct Collateral)`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: TokenHolder.getCollateralExposure(address) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
```
