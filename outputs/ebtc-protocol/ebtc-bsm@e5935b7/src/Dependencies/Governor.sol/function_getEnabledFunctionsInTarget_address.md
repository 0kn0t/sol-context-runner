# Function: getEnabledFunctionsInTarget(address)

**Contract**: [src/Dependencies/Governor.sol/contract_Governor.md]

## Metadata

- **Contract**: Governor
- **Signature**: `getEnabledFunctionsInTarget(address)`
- **Visibility**: public
- **Source Range**: 5746:408:28

## Implementation

```solidity
/// @notice Retrieves all function signatures enabled for a target address.
///  @param _target The target contract address.
///  @return _funcs An array of function signatures enabled for the target.
function getEnabledFunctionsInTarget(address _target) public view returns (bytes4[] memory _funcs) {
    bytes32[] memory _sigs = enabledFunctionSigsByTarget[_target].values();
    if (_sigs.length > 0) {
        _funcs = new bytes4[](_sigs.length);
        for (uint256 i = 0; i < _sigs.length; ++i) {
            _funcs[i] = bytes4(_sigs[i]);
        }
    }
}
```

## Related Implementations

### values(struct EnumerableSet.Bytes32Set)

- **Kind**: internal
- **Source**: 7765:300:27
- **Link**: `src/Dependencies/EnumerableSet.sol:EnumerableSet:values(struct EnumerableSet.Bytes32Set)`

```solidity
///  @dev Return the entire set in an array
///  WARNING: This operation will copy the entire storage to memory, which can be quite expensive. This is designed
///  to mostly be used by view accessors that are queried without any gas fees. Developers should keep in mind that
///  this function has an unbounded cost, and using it as part of a state-changing function may render the function
///  uncallable if the set grows to a point where copying to memory consumes too much gas to fit in a block.
function values(Bytes32Set storage set) internal view returns (bytes32[] memory) {
    bytes32[] memory store = _values(set._inner);
    bytes32[] memory result;
    /// @solidity memory-safe-assembly
    assembly {
        result := store
    }
    return result;
}
```

### _values(struct EnumerableSet.Set)

- **Kind**: internal
- **Source**: 5570:109:27
- **Link**: `src/Dependencies/EnumerableSet.sol:EnumerableSet:_values(struct EnumerableSet.Set)`

```solidity
///  @dev Return the entire set in an array
///  WARNING: This operation will copy the entire storage to memory, which can be quite expensive. This is designed
///  to mostly be used by view accessors that are queried without any gas fees. Developers should keep in mind that
///  this function has an unbounded cost, and using it as part of a state-changing function may render the function
///  uncallable if the set grows to a point where copying to memory consumes too much gas to fit in a block.
function _values(Set storage set) private view returns (bytes32[] memory) {
    return set._values;
}
```

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: Governor.getEnabledFunctionsInTarget(address) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
  └─ [1] ⚙️ FUNCTION: EnumerableSet.values(struct EnumerableSet.Bytes32Set) (NodeID: 1)
      💬 Args: [enabledFunctionSigsByTarget[_target]]
      👁️  Def: internal
    └─ [2] ⚙️ FUNCTION: EnumerableSet._values(struct EnumerableSet.Set) (NodeID: 2)
        💬 Args: [set._inner]
        👁️  Def: private
```

## Documentation

### Function Documentation

@notice Retrieves all function signatures enabled for a target address.
 @param _target The target contract address.
 @return _funcs An array of function signatures enabled for the target.
