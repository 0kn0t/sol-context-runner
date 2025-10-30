# Contract: Reverter

## Metadata

- **Name**: Reverter
- **Type**: Contract
- **Path**: test/lib/Mocks.sol
- **Documentation**: @notice Always reverts with configurable reason.
   @dev Example: Reverter rev = new Reverter("Custom error");

## State Variables

### reason

```solidity
string public reason
```

## Public/External Functions

### constructor(string)

- **Signature**: `constructor(string)`
- **Visibility**: public
- **Source Range**: 4827:68:41
- **Details**: [function_constructor_string.md](./function_constructor_string.md)

**Signature:**
```solidity
/// @notice Configure revert behavior.
///  @param _reason Revert reason string.
constructor(string memory _reason);
```

### fallback()

- **Signature**: `fallback()`
- **Visibility**: external
- **Source Range**: 4978:59:41
- **Details**: [function_fallback.md](./function_fallback.md)

**Signature:**
```solidity
/// @notice Always reverts.
///  @dev Example: rev.fail(); // reverts
fallback() external payable;
```

### receive()

- **Signature**: `receive()`
- **Visibility**: external
- **Source Range**: 5043:30:41
- **Details**: [function_receive.md](./function_receive.md)

**Signature:**
```solidity
receive() external payable;
```
