# Function: removeWhitelistedToken(address)

**Contract**: [src/BorrowerOperationsV6.sol/contract_BorrowerOperationsV6.md]

## Metadata

- **Contract**: BorrowerOperationsV6
- **Signature**: `removeWhitelistedToken(address)`
- **Visibility**: external
- **Source Range**: 3433:149:12

## Implementation

```solidity
function removeWhitelistedToken(address token) external onlyAdmin() {
    whitelistedTokens[token] = false;
    emit TokenRemoved(token);
}
```

## Related Implementations

### onlyAdmin()

- **Kind**: modifier
- **Source**: 2249:114:12
- **Link**: `src/BorrowerOperationsV6.sol:BorrowerOperationsV6:onlyAdmin()`

```solidity
modifier onlyAdmin() {
    require(msg.sender == admin, "Only admin can call this function");
    _;
}
```

## State Variable Reads

- **admin** (`address`)

## State Variable Writes

- **whitelistedTokens** (`mapping(address => bool)`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: BorrowerOperationsV6.removeWhitelistedToken(address) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: external
  └─ [1] 🔒 MODIFIER: BorrowerOperationsV6.onlyAdmin() (NodeID: 1)
      💬 Args: [no args]
```
