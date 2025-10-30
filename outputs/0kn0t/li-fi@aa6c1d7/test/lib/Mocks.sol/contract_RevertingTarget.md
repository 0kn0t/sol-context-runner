# Contract: RevertingTarget

## Metadata

- **Name**: RevertingTarget
- **Type**: Contract
- **Path**: test/lib/Mocks.sol
- **Documentation**: @notice Target that always reverts.
   @dev Example: RevertingTarget target = new RevertingTarget();

## Errors

### AlwaysFails

```solidity
error AlwaysFails(string message);
```

## Public/External Functions

### fail()

- **Signature**: `fail()`
- **Visibility**: public
- **Source Range**: 5743:94:41
- **Details**: [function_fail.md](./function_fail.md)

**Signature:**
```solidity
/// @notice Always reverts with error.
///  @dev Example: target.fail(); // reverts
function fail() public pure;
```

### alwaysReverts()

- **Signature**: `alwaysReverts()`
- **Visibility**: external
- **Source Range**: 5945:80:41
- **Details**: [function_alwaysReverts.md](./function_alwaysReverts.md)

**Signature:**
```solidity
/// @notice Always reverts with message.
///  @dev Example: target.alwaysReverts(); // reverts
function alwaysReverts() external pure;
```
