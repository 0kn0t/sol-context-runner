# Function: replaceParts(uint256,address)

**Contract**: [test/lib/Mocks.sol/contract_SurgeryTarget.md]

## Metadata

- **Contract**: SurgeryTarget
- **Signature**: `replaceParts(uint256,address)`
- **Visibility**: external
- **Source Range**: 5342:129:41

## Implementation

```solidity
/// @notice Return modified values.
///  @dev Example: (uint256, address) = target.replaceParts(123, addr);
function replaceParts(uint256 value, address addr) external pure returns (uint256, address) {
    return (value, addr);
}
```

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: SurgeryTarget.replaceParts(uint256,address) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: external
```

## Documentation

### Function Documentation

@notice Return modified values.
 @dev Example: (uint256, address) = target.replaceParts(123, addr);
