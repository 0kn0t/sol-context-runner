# Contract: ValueCheck

## Metadata

- **Name**: ValueCheck
- **Type**: Contract
- **Path**: test/lib/Mocks.sol
- **Documentation**: @notice Reverts unless msg.value equals expected amount.
   @dev Example: ValueCheck checker = new ValueCheck(1 ether);

## State Variables

### expectedValue

```solidity
uint256 public immutable expectedValue
```

## Public/External Functions

### constructor(uint256)

- **Signature**: `constructor(uint256)`
- **Visibility**: public
- **Source Range**: 4238:83:41
- **Details**: [function_constructor_uint256.md](./function_constructor_uint256.md)

**Signature:**
```solidity
/// @notice Set expected value on deployment.
///  @param _expectedValue Required msg.value for calls.
constructor(uint256 _expectedValue);
```

### fallback()

- **Signature**: `fallback()`
- **Visibility**: external
- **Source Range**: 4433:99:41
- **Details**: [function_fallback.md](./function_fallback.md)

**Signature:**
```solidity
/// @notice Revert if msg.value doesn't match.
///  @dev Example: checker.check{value: 1 ether}();
fallback() external payable;
```

### receive()

- **Signature**: `receive()`
- **Visibility**: external
- **Source Range**: 4538:30:41
- **Details**: [function_receive.md](./function_receive.md)

**Signature:**
```solidity
receive() external payable;
```
