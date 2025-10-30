# Function: executeCall(address,bytes)

**Contract**: [test/lib/Mocks.sol/contract_DispatcherTarget.md]

## Metadata

- **Contract**: DispatcherTarget
- **Signature**: `executeCall(address,bytes)`
- **Visibility**: external
- **Source Range**: 6633:226:41

## Implementation

```solidity
/// @notice Execute call to target with data.
///  @dev Example: bytes memory result = dispatcher.executeCall(target, data);
function executeCall(address target, bytes calldata data) external returns (bytes memory) {
    (bool success, bytes memory result) = target.call(data);
    require(success, "Call failed");
    return result;
}
```

## External Calls

- **address::call(bytes calldata)**

## Native Transfers

- **target** (function parameter)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: DispatcherTarget.executeCall(address,bytes) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: external
```

## Documentation

### Function Documentation

@notice Execute call to target with data.
 @dev Example: bytes memory result = dispatcher.executeCall(target, data);
