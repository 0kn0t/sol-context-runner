# Contract: VMLogLib

## Metadata

- **Name**: VMLogLib
- **Type**: Contract
- **Path**: src/VMLogLib.sol
- **Documentation**: @title VMLogLib
   @notice Library for handling the LOG operation that emits events with raw bytes data.

## Events

### VMLogStatic1

```solidity
/// @notice Event emitted for static data with 1 register (32 bytes)
event VMLogStatic1(bytes32 data1);
```

### VMLogStatic2

```solidity
/// @notice Event emitted for static data with 2 registers (64 bytes)
event VMLogStatic2(bytes32 data1, bytes32 data2);
```

### VMLogStatic3

```solidity
/// @notice Event emitted for static data with 3 registers (96 bytes)
event VMLogStatic3(bytes32 data1, bytes32 data2, bytes32 data3);
```

### VMLogStatic4

```solidity
/// @notice Event emitted for static data with 4 registers (128 bytes)
event VMLogStatic4(bytes32 data1, bytes32 data2, bytes32 data3, bytes32 data4);
```

### VMLogStatic5

```solidity
/// @notice Event emitted for static data with 5 registers (160 bytes)
event VMLogStatic5(bytes32 data1, bytes32 data2, bytes32 data3, bytes32 data4, bytes32 data5);
```

### VMLogDyn

```solidity
/// @notice Event emitted for dynamic data from a register
event VMLogDyn(bytes data);
```
