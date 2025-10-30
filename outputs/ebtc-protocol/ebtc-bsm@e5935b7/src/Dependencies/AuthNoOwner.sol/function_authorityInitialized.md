# Function: authorityInitialized()

**Contract**: [src/Dependencies/AuthNoOwner.sol/contract_AuthNoOwner.md]

## Metadata

- **Contract**: AuthNoOwner
- **Signature**: `authorityInitialized()`
- **Visibility**: public
- **Source Range**: 871:104:25

## Implementation

```solidity
function authorityInitialized() public view returns (bool) {
    return _authorityInitialized;
}
```

## State Variable Reads

- **_authorityInitialized** (`bool`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: AuthNoOwner.authorityInitialized() (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
```
