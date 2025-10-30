# Contract: ArithmeticProcessor

## Metadata

- **Name**: ArithmeticProcessor
- **Type**: Contract
- **Path**: src/RPNArithmetic.sol

## State Variables

### PUSH_REG_FLAG

```solidity
uint8 internal constant PUSH_REG_FLAG = 0x80
```

### REG_INDEX_MASK

```solidity
uint8 internal constant REG_INDEX_MASK = 0x7F
```

### OP_ADD

```solidity
uint8 internal constant OP_ADD = uint8(OpCode.ADD)
```

### OP_SUB

```solidity
uint8 internal constant OP_SUB = uint8(OpCode.SUB)
```

### OP_MUL

```solidity
uint8 internal constant OP_MUL = uint8(OpCode.MUL)
```

### OP_DIV

```solidity
uint8 internal constant OP_DIV = uint8(OpCode.DIV)
```

## Errors

### EmptyExpression

```solidity
error EmptyExpression();
```

### RegIndexOOB

```solidity
error RegIndexOOB(uint8 attemptedRegIndex);
```

### StackUnderflow

```solidity
error StackUnderflow();
```

### DivByZero

```solidity
error DivByZero();
```

### InvalidRPNStack

```solidity
error InvalidRPNStack();
```

### MissingDestReg

```solidity
error MissingDestReg();
```

### TooManyOpcodes

```solidity
error TooManyOpcodes(uint8 actualLen, uint8 maxLen);
```

### InvalidOpcode

```solidity
error InvalidOpcode(uint8 opcode);
```

## Enums

### OpCode

```solidity
enum OpCode {
    ADD,
    SUB,
    MUL,
    DIV
}
```

## Public/External Functions

### evaluateRPN(uint256[],bytes32,uint8)

- **Signature**: `evaluateRPN(uint256[],bytes32,uint8)`
- **Visibility**: external
- **Source Range**: 944:2607:29
- **Details**: [function_evaluateRPN_uint256[]_bytes32_uint8.md](./function_evaluateRPN_uint256[]_bytes32_uint8.md)

**Signature:**
```solidity
function evaluateRPN(uint256[] calldata regValues, bytes32 rpnStream, uint8 rpnLen) external pure returns (uint256 result);
```
