# Function: paused()

**Contract**: [src/EbtcBSM.sol/contract_EbtcBSM.md]

## Metadata

- **Contract**: EbtcBSM
- **Signature**: `paused()`
- **Visibility**: public
- **Source Range**: 1850:84:15
- **Inherited From**: Pausable

## Implementation

```solidity
///  @dev Returns true if the contract is paused, and false otherwise.
function paused() virtual public view returns (bool) {
    return _paused;
}
```

## State Variable Reads

- **_paused** (`bool`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: Pausable.paused() (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
```

## Documentation

### Function Documentation

 @dev Returns true if the contract is paused, and false otherwise.
