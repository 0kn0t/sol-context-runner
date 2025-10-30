# Function: batchAssertPacked(uint256,uint8,uint256[])

**Contract**: [src/InvariantChecker.sol/contract_InvariantChecker.md]

## Metadata

- **Contract**: InvariantChecker
- **Signature**: `batchAssertPacked(uint256,uint8,uint256[])`
- **Visibility**: external
- **Source Range**: 6403:435:27

## Implementation

```solidity
///  @notice Gas-optimized batch check using packed opcodes in a single uint256
///  @param packedOps A uint256 with packed operation codes, each taking 8 bits (0=nop, 1=eq, 2=neq, 3=lt, 4=gt, 5=lte, 6=gte, 7=range)
///  @param opCount Number of operations packed into packedOps (max 32)
///  @param values Array of values to check (for range check: [value, min, max])
///  @dev Op 0 (nop) performs no operation and doesn't consume any values
///  @dev For ops 1-6: values[valueIdx] and values[valueIdx+1] are compared
///  @dev For op 7: values[valueIdx], values[valueIdx+1], and values[valueIdx+2] are used
///  @dev Note: valueIdx is dynamically updated based on the operations executed
function batchAssertPacked(uint256 packedOps, uint8 opCount, uint256[] calldata values) external pure {
    if (opCount > 32) revert VmErrors.TooManyOperations();
    uint256 valueIdx = 0;
    for (uint8 i = 0; i < opCount; ) {
        uint8 op = uint8(packedOps >> (248 - (i * 8)));
        valueIdx = _checkAssertion(op, values, valueIdx);
        unchecked {
            ++i;
        }
    }
}
```

## Related Implementations

### _checkAssertion(uint8,uint256[],uint256)

- **Kind**: internal
- **Source**: 2731:1992:27
- **Link**: `src/InvariantChecker.sol:InvariantChecker:_checkAssertion(uint8,uint256[],uint256)`

```solidity
///  @notice Internal function to check a single assertion based on operation code
///  @param op Operation code (0=nop, 1=eq, 2=neq, 3=lt, 4=gt, 5=lte, 6=gte, 7=range)
///  @param values Array of values to check
///  @param valueIdx Current index in the values array
///  @return nextIdx The next index to use in the values array
function _checkAssertion(uint8 op, uint256[] calldata values, uint256 valueIdx) internal pure returns (uint256 nextIdx) {
    uint256 val1 = values[valueIdx];
    uint256 val2;
    unchecked {
        if (op == 1) {
            val2 = values[valueIdx + 1];
            if (val1 != val2) revert AssertEqFailed(val1, val2);
            return valueIdx + 2;
        } else if (op == 2) {
            val2 = values[valueIdx + 1];
            if (val1 == val2) revert AssertNeqFailed(val1, val2);
            return valueIdx + 2;
        } else if (op == 3) {
            val2 = values[valueIdx + 1];
            if (val1 >= val2) revert AssertLtFailed(val1, val2);
            return valueIdx + 2;
        } else if (op == 4) {
            val2 = values[valueIdx + 1];
            if (val1 <= val2) revert AssertGtFailed(val1, val2);
            return valueIdx + 2;
        } else if (op == 5) {
            val2 = values[valueIdx + 1];
            if (val1 > val2) revert AssertLteFailed(val1, val2);
            return valueIdx + 2;
        } else if (op == 6) {
            val2 = values[valueIdx + 1];
            if (val1 < val2) revert AssertGteFailed(val1, val2);
            return valueIdx + 2;
        } else if (op == 7) {
            val2 = values[valueIdx + 1];
            uint256 val3 = values[valueIdx + 2];
            if ((val1 < val2) || (val1 > val3)) revert AssertRangeFailed(val1, val2, val3);
            return valueIdx + 3;
        } else if (op != 0) {
            revert UnknownOpcode(op);
        }
    }
    return valueIdx;
}
```

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: InvariantChecker.batchAssertPacked(uint256,uint8,uint256[]) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: external
  └─ [1] ⚙️ FUNCTION: InvariantChecker._checkAssertion(uint8,uint256[],uint256) (NodeID: 1)
      💬 Args: [op, values, valueIdx]
      👁️  Def: internal
```

## Documentation

### Function Documentation

 @notice Gas-optimized batch check using packed opcodes in a single uint256
 @param packedOps A uint256 with packed operation codes, each taking 8 bits (0=nop, 1=eq, 2=neq, 3=lt, 4=gt, 5=lte, 6=gte, 7=range)
 @param opCount Number of operations packed into packedOps (max 32)
 @param values Array of values to check (for range check: [value, min, max])
 @dev Op 0 (nop) performs no operation and doesn't consume any values
 @dev For ops 1-6: values[valueIdx] and values[valueIdx+1] are compared
 @dev For op 7: values[valueIdx], values[valueIdx+1], and values[valueIdx+2] are used
 @dev Note: valueIdx is dynamically updated based on the operations executed
