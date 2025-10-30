# Function: setPrice(uint256)

**Contract**: [src/mocks/OracleMock.sol/contract_OracleMock.md]

## Metadata

- **Contract**: OracleMock
- **Signature**: `setPrice(uint256)`
- **Visibility**: external
- **Source Range**: 186:78:20

## Implementation

```solidity
function setPrice(uint256 newPrice) external {
    price = newPrice;
}
```

## State Variable Writes

- **price** (`uint256`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: OracleMock.setPrice(uint256) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: external
```
