# Function: enableIrm(address)

**Contract**: [src/Morpho.sol/contract_Morpho.md]

## Metadata

- **Contract**: Morpho
- **Signature**: `enableIrm(address)`
- **Visibility**: external
- **Source Range**: 3093:193:0

## Implementation

```solidity
/// @inheritdoc IMorphoBase
function enableIrm(address irm) external onlyOwner() {
    require(!isIrmEnabled[irm], ErrorsLib.ALREADY_SET);
    isIrmEnabled[irm] = true;
    emit EventsLib.EnableIrm(irm);
}
```

## Related Implementations

### onlyOwner()

- **Kind**: modifier
- **Source**: 2695:98:0
- **Link**: `src/Morpho.sol:Morpho:onlyOwner()`

```solidity
/// @dev Reverts if the caller is not the owner.
modifier onlyOwner() {
    require(msg.sender == owner, ErrorsLib.NOT_OWNER);
    _;
}
```

## State Variable Reads

- **isIrmEnabled** (`mapping(address => bool)`)
- **owner** (`address`)

## State Variable Writes

- **isIrmEnabled** (`mapping(address => bool)`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: Morpho.enableIrm(address) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: external
  └─ [1] 🔒 MODIFIER: Morpho.onlyOwner() (NodeID: 1)
      💬 Args: [no args]
```

## Documentation

### Function Documentation

@inheritdoc IMorphoBase

### Interface Documentation

@notice Enables `irm` as a possible IRM for market creation.
 @dev Warning: It is not possible to disable an IRM.
