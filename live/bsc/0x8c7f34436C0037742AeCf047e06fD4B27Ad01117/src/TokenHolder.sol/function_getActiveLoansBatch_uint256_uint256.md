# Function: getActiveLoansBatch(uint256,uint256)

**Contract**: [src/TokenHolder.sol/contract_TokenHolder.md]

## Metadata

- **Contract**: TokenHolder
- **Signature**: `getActiveLoansBatch(uint256,uint256)`
- **Visibility**: public
- **Source Range**: 7292:613:13

## Implementation

```solidity
function getActiveLoansBatch(uint256 startIndex, uint256 batchSize) public view returns (Loan[] memory) {
    uint256 endIndex = startIndex + batchSize;
    if (endIndex > activeLoanIds.length) {
        endIndex = activeLoanIds.length;
    }
    uint256 resultSize = endIndex - startIndex;
    Loan[] memory result = new Loan[](resultSize);
    for (uint256 i = 0; i < resultSize; i++) {
        uint256 loanId = activeLoanIds[startIndex + i];
        result[i] = loans[loanId];
    }
    return result;
}
```

## State Variable Reads

- **activeLoanIds** (`uint256[]`)
- **loans** (`mapping(uint256 => struct Loan)`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: TokenHolder.getActiveLoansBatch(uint256,uint256) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
```
