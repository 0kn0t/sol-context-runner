# Contract: DispatcherTarget

## Metadata

- **Name**: DispatcherTarget
- **Type**: Contract
- **Path**: test/lib/Mocks.sol
- **Documentation**: @notice Dispatcher executing calls to arbitrary addresses.
   @dev Example: DispatcherTarget dispatcher = new DispatcherTarget();

## Public/External Functions

### executeCall(address,bytes)

- **Signature**: `executeCall(address,bytes)`
- **Visibility**: external
- **Source Range**: 6633:226:41
- **Details**: [function_executeCall_address_bytes.md](./function_executeCall_address_bytes.md)

**Signature:**
```solidity
/// @notice Execute call to target with data.
///  @dev Example: bytes memory result = dispatcher.executeCall(target, data);
function executeCall(address target, bytes calldata data) external returns (bytes memory);
```
