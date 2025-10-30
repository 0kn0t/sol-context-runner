# Contract: RegisterFile

## Metadata

- **Name**: RegisterFile
- **Type**: Contract
- **Path**: src/RegisterFile.sol
- **Documentation**: @title Register file utilities for the VM

## State Variables

### ZERO_VALUE

```solidity
/// @dev ABI‑encoded `uint256(0)` for the void register.
bytes internal constant ZERO_VALUE = hex"0000000000000000000000000000000000000000000000000000000000000000"
```

## Errors

### RegisterIndexOOB

```solidity
/// @notice Thrown when `index` ≥ `registers.length`.
error RegisterIndexOOB();
```

### InvalidDynamicData

```solidity
/// @notice Thrown when provided data is not a dynamic abi-encoded field.
error InvalidDynamicData();
```

### InvalidStaticData

```solidity
/// @notice Thrown when provided data is not a static abi-encoded field (32 bytes).
error InvalidStaticData();
```
