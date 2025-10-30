# Contract: VmErrors

## Metadata

- **Name**: VmErrors
- **Type**: Contract
- **Path**: src/VmErrors.sol
- **Documentation**: @title VmErrors
   @notice Shared error definitions for the Virtual Machine

## Errors

### Disallowed

```solidity
/// @notice Thrown when a disallowed operation is executed
error Disallowed();
```

### OutOfBounds

```solidity
/// @notice Thrown when a surgery operation exceeds the bounds of the source data
error OutOfBounds();
```

### ReplacementTooLarge

```solidity
/// @notice Thrown when replacement data exceeds surgery length
error ReplacementTooLarge();
```

### TooManySurgeries

```solidity
/// @notice Thrown when too many surgeries are requested (maximum 6)
error TooManySurgeries();
```

### InvalidLogVariant

```solidity
/// @notice Thrown when an invalid log variant is provided
error InvalidLogVariant();
```

### InvalidCallType

```solidity
/// @notice Thrown when an invalid call type is provided
error InvalidCallType();
```

### BlueprintTooLarge

```solidity
/// @notice Thrown when a blueprint exceeds the maximum allowed size
error BlueprintTooLarge();
```

### SourceRegistersExceed208Bits

```solidity
/// @notice Thrown when source registers exceed 208 bits for log packing
error SourceRegistersExceed208Bits();
```

### NoApprovedTokens

```solidity
/// @notice Thrown when no tokens are approved for deposit
error NoApprovedTokens();
```

### TokenTransferFailed

```solidity
/// @notice Thrown when token transfer fails during deposit
error TokenTransferFailed();
```

### InvalidOpcode

```solidity
/// @notice Thrown when an invalid opcode is encountered
error InvalidOpcode(uint8 opcode);
```

### InvalidAddressBytes

```solidity
/// @notice Thrown when bytes are attempted to be read as address
error InvalidAddressBytes();
```

### InvalidCallDataLength

```solidity
/// @notice Thrown when calldata is too short (must be at least 36 bytes: 32 for length + 4 for selector)
error InvalidCallDataLength();
```

### InvalidValueLength

```solidity
/// @notice Thrown when value data is too short (must be at least 32 bytes)
error InvalidValueLength();
```

### TooManyOperations

```solidity
/// @notice Thrown when too many operations are requested (maximum 32)
error TooManyOperations();
```

### InvalidUserAddress

```solidity
/// @notice Thrown when an invalid user address (zero address) is provided
error InvalidUserAddress();
```

### ProxyAlreadyDeployed

```solidity
/// @notice Thrown when a proxy has already been deployed for the user
error ProxyAlreadyDeployed();
```

### Unauthorized

```solidity
/// @notice Thrown when unauthorized access is attempted
error Unauthorized();
```
