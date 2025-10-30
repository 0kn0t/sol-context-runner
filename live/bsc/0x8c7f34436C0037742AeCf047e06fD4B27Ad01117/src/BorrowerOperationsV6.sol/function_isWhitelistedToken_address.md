# Function: isWhitelistedToken(address)

**Contract**: [src/BorrowerOperationsV6.sol/contract_BorrowerOperationsV6.md]

## Metadata

- **Contract**: BorrowerOperationsV6
- **Signature**: `isWhitelistedToken(address)`
- **Visibility**: external
- **Source Range**: 3588:120:12

## Implementation

```solidity
function isWhitelistedToken(address token) external view returns (bool) {
    return whitelistedTokens[token];
}
```

## State Variable Reads

- **whitelistedTokens** (`mapping(address => bool)`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: BorrowerOperationsV6.isWhitelistedToken(address) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: external
```
