# Contract: VmConstants

## Metadata

- **Name**: VmConstants
- **Type**: Contract
- **Path**: src/VmConstants.sol
- **Documentation**: @title VmConstants
   @notice Shared constants for the Virtual Machine and test helpers

## State Variables

### DYN_MASK

```solidity
uint8 public constant DYN_MASK = 0x80
```

### IDX_MASK

```solidity
uint8 public constant IDX_MASK = 0x7F
```

### VOID_REG

```solidity
uint8 public constant VOID_REG = 0x7A
```

### MAX_SURGERIES

```solidity
uint8 public constant MAX_SURGERIES = 6
```

### MAX_CDB_BP

```solidity
uint8 public constant MAX_CDB_BP = 22
```

### MAX_ABI_BP

```solidity
uint8 public constant MAX_ABI_BP = 27
```

### START_TUPLE_STATIC

```solidity
uint8 public constant START_TUPLE_STATIC = 0x7F
```

### START_TUPLE_DYNAMIC

```solidity
uint8 public constant START_TUPLE_DYNAMIC = 0x7E
```

### START_ARRAY_STATIC

```solidity
uint8 public constant START_ARRAY_STATIC = 0x7D
```

### START_ARRAY_DYNAMIC

```solidity
uint8 public constant START_ARRAY_DYNAMIC = 0x7C
```

### END_DYNAMIC

```solidity
uint8 public constant END_DYNAMIC = 0x7B
```
