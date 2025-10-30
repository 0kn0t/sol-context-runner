# Function: evaluateRPN(uint256[],bytes32,uint8)

**Contract**: [src/RPNArithmetic.sol/contract_ArithmeticProcessor.md]

## Metadata

- **Contract**: ArithmeticProcessor
- **Signature**: `evaluateRPN(uint256[],bytes32,uint8)`
- **Visibility**: external
- **Source Range**: 944:2607:29

## Implementation

```solidity
function evaluateRPN(uint256[] calldata regValues, bytes32 rpnStream, uint8 rpnLen) external pure returns (uint256 result) {
    uint256 regsLen = regValues.length;
    if (rpnLen > 32) revert TooManyOpcodes(rpnLen, 32);
    uint256[] memory stack = new uint256[](rpnLen);
    uint8 sp = 0;
    for (uint8 pc = 0; pc < rpnLen; ) {
        uint8 op = uint8(uint256(rpnStream >> ((31 - uint256(pc)) * 8)));
        if ((op & PUSH_REG_FLAG) != 0) {
            uint8 regIndex = op & REG_INDEX_MASK;
            if (regIndex >= regsLen) {
                revert RegIndexOOB(regIndex);
            }
            uint256 val = regValues[regIndex];
            if (sp >= rpnLen) {
                revert InvalidRPNStack();
            }
            stack[sp] = val;
            unchecked {
                ++sp;
            }
        } else {
            if (sp < 2) {
                revert StackUnderflow();
            }
            uint256 a;
            uint256 b;
            unchecked {
                sp -= 2;
                a = stack[sp];
                b = stack[sp + 1];
            }
            uint256 c;
            if (op == OP_ADD) {
                c = a + b;
            } else if (op == OP_SUB) {
                c = a - b;
            } else if (op == OP_MUL) {
                c = a * b;
            } else if (op == OP_DIV) {
                if (b == 0) {
                    revert DivByZero();
                }
                c = a / b;
            } else {
                revert InvalidOpcode(op);
            }
            stack[sp] = c;
            unchecked {
                ++sp;
            }
        }
        unchecked {
            ++pc;
        }
    }
    if (regsLen == 0) {
        revert MissingDestReg();
    }
    if (sp != 1) {
        revert InvalidRPNStack();
    }
    result = stack[0];
    return result;
}
```

## State Variable Reads

- **PUSH_REG_FLAG** (`uint8`)
- **REG_INDEX_MASK** (`uint8`)
- **OP_ADD** (`uint8`)
- **OP_SUB** (`uint8`)
- **OP_MUL** (`uint8`)
- **OP_DIV** (`uint8`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: ArithmeticProcessor.evaluateRPN(uint256[],bytes32,uint8) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: external
```
