# Function: encodeFromBlueprint(bytes4,bytes,bytes[])

**Contract**: [src/BlueprintEncoder.sol/contract_BlueprintEncoder.md]

## Metadata

- **Contract**: BlueprintEncoder
- **Signature**: `encodeFromBlueprint(bytes4,bytes,bytes[])`
- **Visibility**: public
- **Source Range**: 26673:1950:22

## Implementation

```solidity
///  @notice Encodes a full ABI payload, prepending a 4-byte function selector.
///  @param selector The function selector.
///  @param blueprint The byte-level layout description.
///  @param registers Caller-supplied data referenced by the blueprint.
///  @return out A (4 + N)-byte ABI blob suitable for a low-level `call`.
function encodeFromBlueprint(bytes4 selector, bytes calldata blueprint, bytes[] memory registers) public pure returns (bytes memory out) {
    if (blueprint.length == 0) {
        out = _allocWithLength(4);
        assembly {
            mstore(add(out, 0x40), selector)
        }
        return out;
    }
    if (_isFlat(blueprint)) {
        (uint256 headB, uint256 tailB) = _measureFlat(blueprint, registers);
        return _encodeFlat(selector, blueprint, registers, headB, tailB);
    }
    uint256[10] memory tmpHeads;
    (uint256 total, uint256 headBytes, uint256 dynCnt) = _measure(blueprint, registers, tmpHeads);
    uint256[] memory dynHead = new uint256[](dynCnt);
    for (uint256 j; j < dynCnt; ) {
        dynHead[j] = tmpHeads[j];
        unchecked {
            ++j;
        }
    }
    uint256 payloadLen = 4 + total;
    out = _allocWithLength(payloadLen);
    assembly {
        mstore(add(out, 0x40), selector)
    }
    _encodeCore(blueprint, registers, dynHead, out, 32 + 4, headBytes);
    return out;
}
```

## Call Tree

```
No call tree available
```

## Documentation

### Function Documentation

 @notice Encodes a full ABI payload, prepending a 4-byte function selector.
 @param selector The function selector.
 @param blueprint The byte-level layout description.
 @param registers Caller-supplied data referenced by the blueprint.
 @return out A (4 + N)-byte ABI blob suitable for a low-level `call`.
