# Contract: SurgeryTarget

## Metadata

- **Name**: SurgeryTarget
- **Type**: Contract
- **Path**: test/lib/Mocks.sol
- **Documentation**: @notice Target for testing CALLDATA_SURGERY operation.
   @dev Example: SurgeryTarget target = new SurgeryTarget();

## Public/External Functions

### replaceParts(uint256,address)

- **Signature**: `replaceParts(uint256,address)`
- **Visibility**: external
- **Source Range**: 5342:129:41
- **Details**: [function_replaceParts_uint256_address.md](./function_replaceParts_uint256_address.md)

**Signature:**
```solidity
/// @notice Return modified values.
///  @dev Example: (uint256, address) = target.replaceParts(123, addr);
function replaceParts(uint256 value, address addr) external pure returns (uint256, address);
```
