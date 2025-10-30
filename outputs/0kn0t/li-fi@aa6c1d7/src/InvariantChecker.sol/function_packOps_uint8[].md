# Function: packOps(uint8[])

**Contract**: [src/InvariantChecker.sol/contract_InvariantChecker.md]

## Metadata

- **Contract**: InvariantChecker
- **Signature**: `packOps(uint8[])`
- **Visibility**: external
- **Source Range**: 7117:373:27

## Implementation

```solidity
///  @notice Helper function to pack operation codes into a single uint256
///  @param ops Array of operation codes to pack
///  @return packed The packed uint256 with all operation codes
///  @dev This is a pure view function primarily for testing
function packOps(uint8[] memory ops) external pure returns (uint256 packed) {
    if (ops.length > 32) revert VmErrors.TooManyOperations();
    uint256 opsLen = ops.length;
    packed = 0;
    for (uint8 i = 0; i < opsLen; ) {
        packed |= uint256(ops[i]) << (248 - (i * 8));
        unchecked {
            ++i;
        }
    }
}
```

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: InvariantChecker.packOps(uint8[]) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: external
```

## Documentation

### Function Documentation

 @notice Helper function to pack operation codes into a single uint256
 @param ops Array of operation codes to pack
 @return packed The packed uint256 with all operation codes
 @dev This is a pure view function primarily for testing
