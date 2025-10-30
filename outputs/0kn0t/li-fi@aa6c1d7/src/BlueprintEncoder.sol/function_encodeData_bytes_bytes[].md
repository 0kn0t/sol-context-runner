# Function: encodeData(bytes,bytes[])

**Contract**: [src/BlueprintEncoder.sol/contract_BlueprintEncoder.md]

## Metadata

- **Contract**: BlueprintEncoder
- **Signature**: `encodeData(bytes,bytes[])`
- **Visibility**: public
- **Source Range**: 29064:1264:22

## Implementation

```solidity
///  @notice Encodes an ABI data blob *without* a function selector.
///  @param blueprint Layout description.
///  @param registers Caller-supplied registers.
///  @return out ABI-encoded data blob.
function encodeData(bytes calldata blueprint, bytes[] memory registers) public pure returns (bytes memory out) {
    if (blueprint.length == 0) return _allocWithLength(0);
    if (_isFlat(blueprint)) {
        (uint256 headB, uint256 tailB) = _measureFlat(blueprint, registers);
        uint256 payloadLen = headB + tailB;
        out = _allocWithLength(payloadLen);
        _encodeFlatCoreInto(out, blueprint, registers, headB, 32);
        return out;
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
    out = _allocWithLength(total);
    _encodeCore(blueprint, registers, dynHead, out, 32, headBytes);
    return out;
}
```

## Call Tree

```
No call tree available
```

## Documentation

### Function Documentation

 @notice Encodes an ABI data blob *without* a function selector.
 @param blueprint Layout description.
 @param registers Caller-supplied registers.
 @return out ABI-encoded data blob.
