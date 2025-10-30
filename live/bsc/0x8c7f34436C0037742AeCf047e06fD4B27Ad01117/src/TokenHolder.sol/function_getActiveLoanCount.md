# Function: getActiveLoanCount()

**Contract**: [src/TokenHolder.sol/contract_TokenHolder.md]

## Metadata

- **Contract**: TokenHolder
- **Signature**: `getActiveLoanCount()`
- **Visibility**: public
- **Source Range**: 7106:104:13

## Implementation

```solidity
function getActiveLoanCount() public view returns (uint256) {
    return activeLoanIds.length;
}
```

## State Variable Reads

- **activeLoanIds** (`uint256[]`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: TokenHolder.getActiveLoanCount() (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
```
