# Function: authorityInitialized()

**Contract**: [src/BaseEscrow.sol/contract_BaseEscrow.md]

## Metadata

- **Contract**: BaseEscrow
- **Signature**: `authorityInitialized()`
- **Visibility**: public
- **Source Range**: 871:104:25
- **Inherited From**: AuthNoOwner

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
