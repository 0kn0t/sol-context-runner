# Contract: BlueprintEncoder

## Metadata

- **Name**: BlueprintEncoder
- **Type**: Contract
- **Path**: src/BlueprintEncoder.sol
- **Documentation**:  @title  BlueprintEncoder
   @notice A specialized, gas-efficient ABI encoder that uses a custom Domain
   Specific Language (DSL) called a "blueprint" to construct calldata.
   @dev    This library is not a general-purpose ABI encoder. It is designed for
   applications where the structure of the calldata is
   known in advance. The caller must prepare all data in `registers`
   beforehand and provide a `blueprint` that dictates the final layout.
   CONCEPT OVERVIEW:
   1. Blueprint: A `bytes` array that acts as a set of instructions. It contains
   a sequence of tokens that define the structure of the output.
   2. Registers: An array of `bytes` (`bytes[]`) that serves as a data source.
   The blueprint references items in this array by their index. Each register
   must be pre-formatted according to its type (static or dynamic).
   3. Tokens: The blueprint is composed of two categories of tokens (bytes):
   A. Register Tokens (0x00 - 0x79): These bytes are indices into the
   `registers` array. The 7th bit acts as a flag:
   - Bit 7 clear (0b0...): The token points to a *static* register, which
   is expected to be exactly 32 bytes long.
   - Bit 7 set   (0b1...): The token points to a *dynamic* register. The
   register itself must be a self-contained, length-prefixed ABI blob
   (i.e., `length | data | padding`).
   B. Container Tokens (0x7B - 0x7F): These are control tokens that define
   the start and end of tuples and arrays, allowing for nested data
   structures.
   ENCODING PROCESS:
   The encoder uses a two-pass system:
   - Fast Path (Flat Blueprint): If the blueprint contains no container tokens,
   the encoder performs a single pass. It calculates the total size and writes
   the data in one go, minimizing overhead.
   - General Case (Nested Blueprint): For blueprints with nested structures,
   a two-pass "measure-then-write" approach is used:
   1. Measure Pass (`_measure`): Traverses the blueprint to calculate the
   exact size of the head and tail sections of the final payload without
   writing any data. It uses a stack to handle nested containers and
   records the dimensions of dynamic structures.
   2. Encode Pass (`_encodeCore`): Allocates a single buffer of the exact
   required size and then performs a second traversal of the blueprint to
   write the data, using the measurements from the first pass to correctly
   place all pointers and data.
   This two-pass system ensures that the entire payload can be constructed with
   a single memory allocation.
   HOW TO USE THE BLUEPRINT DSL:
   The blueprint DSL is designed for applications where you need to construct
   ABI-encoded calldata or data blobs from pre-prepared register values. Here's
   how to use it effectively:
   1. **Prepare Your Data in Registers**: Before creating a blueprint, store all
   your data in the `registers` array. Each register must be properly formatted:
   - Static registers: Exactly 32 bytes (e.g., `abi.encode(uint256)`)
   - Dynamic registers: First `abi.encode()` the data, then use `setDynamic()`
     to create the length-prefixed blob
   2. **Construct the Blueprint**: Create a byte sequence using tokens:
   - `0x00-0x79`: Static register indices (bit 7 clear)
   - `0x80-0xF9`: Dynamic register indices (bit 7 set, actual index = value & 0x7F)
   - `0x7F`: Start static tuple `(...)`
   - `0x7E`: Start dynamic tuple `(...)`
   - `0x7D`: Start static array `[...]`
   - `0x7C`: Start dynamic array `[...]`
   - `0x7B`: End any container
   **Example 1 - Simple Function Call:**
   ```solidity
   // Function: transfer(address to, uint256 amount)
   bytes[] memory registers = new bytes[](2);
   registers[0] = abi.encode(address(0x123...)); // to address
   registers[1] = abi.encode(uint256(1000));     // amount
   bytes memory blueprint = abi.encodePacked(
       uint8(0), // static register 0 (address)
       uint8(1)  // static register 1 (amount)
   );
   bytes memory calldata = BlueprintEncoder.encodeFromBlueprint(
       bytes4(keccak256("transfer(address,uint256)")),
       blueprint,
       registers
   );
   ```
   **Example 2 - Dynamic Data with Nested Structures:**
   ```solidity
   // Function: processData(uint256 id, (uint256 value, bytes data), uint256[] amounts)
   bytes[] memory registers = new bytes[](6);
   registers[0] = abi.encode(uint256(42));           // id
   registers[1] = abi.encode(uint256(100));          // tuple.value
   registers.setDynamic(2, abi.encode(bytes("hello"))); // tuple.data (note: setDynamic after abi.encode)
   registers[3] = abi.encode(uint256(10));           // amounts[0]
   registers[4] = abi.encode(uint256(20));           // amounts[1]
   registers[5] = abi.encode(uint256(30));           // amounts[2]
   bytes memory blueprint = abi.encodePacked(
       uint8(0),    // static register 0 (id)
       uint8(0x7E), // START_TUPLE_DYNAMIC (tuple begins)
           uint8(1),    // static register 1 (tuple.value)
           uint8(0x82), // dynamic register 2 (tuple.data, 2 | 0x80)
       uint8(0x7B), // END_DYNAMIC (tuple ends)
       uint8(0x7C), // START_ARRAY_DYNAMIC (array begins)
           uint8(3),    // static register 3 (amounts[0])
           uint8(4),    // static register 4 (amounts[1])
           uint8(5),    // static register 5 (amounts[2])
       uint8(0x7B)  // END_DYNAMIC (array ends)
   );
   ```

## State Variables

### LEN_WORD

```solidity
uint256 internal constant LEN_WORD = 32
```

### SHIFT_CNT

```solidity
uint256 internal constant SHIFT_CNT = 96
```

### MASK_LEN

```solidity
uint256 internal constant MASK_LEN = (1 << 96) - 1
```

### REG_DYNAMIC_MASK

```solidity
/// Bit mask to check the "dynamic" flag on a register token (0x80 = 10000000).
///  If `(token & REG_DYNAMIC_MASK) != 0`, it's a dynamic register.
uint8 internal constant REG_DYNAMIC_MASK = 0x80
```

### START_TUPLE_STATIC

```solidity
uint8 internal constant START_TUPLE_STATIC = 0x7F
```

### START_TUPLE_DYNAMIC

```solidity
uint8 internal constant START_TUPLE_DYNAMIC = 0x7E
```

### START_ARRAY_STATIC

```solidity
uint8 internal constant START_ARRAY_STATIC = 0x7D
```

### START_ARRAY_DYNAMIC

```solidity
uint8 internal constant START_ARRAY_DYNAMIC = 0x7C
```

### END_DYNAMIC

```solidity
/// A single, shared token to close any container type.
uint8 internal constant END_DYNAMIC = 0x7B
```

### MAX_STACK_DEPTH

```solidity
/// A safety limit to prevent excessively deep nesting, which could lead to
///  stack exhaustion or denial-of-service. 12 levels is a generous limit.
uint8 internal constant MAX_STACK_DEPTH = 12
```

### MASK_48

```solidity
uint256 internal constant MASK_48 = (1 << 48) - 1
```

### MASK_96

```solidity
uint256 internal constant MASK_96 = (1 << 96) - 1
```

## Errors

### StackOverflow

```solidity
error StackOverflow();
```

### StackUnderflow

```solidity
error StackUnderflow();
```

### DynHeadBufOverflow

```solidity
error DynHeadBufOverflow();
```

### InvalidToken

```solidity
error InvalidToken();
```

### UnclosedContainer

```solidity
error UnclosedContainer();
```

### BufferTooSmall

```solidity
error BufferTooSmall(uint256 required, uint256 provided);
```

### BadStaticFormat

```solidity
error BadStaticFormat(uint8 index);
```

### BadDynamicFormat

```solidity
error BadDynamicFormat(uint8 index);
```

## Enums

### ContainerKind

```solidity
///  @dev Identifies the type of container currently being processed on the stack
///  during either the measurement or encoding pass.
enum ContainerKind {
    NONE,
    DYNAMIC_ARRAY,
    DYNAMIC_TUPLE,
    STATIC_ARRAY,
    STATIC_TUPLE
}
```

## Public/External Functions

### encodeFromBlueprint(bytes4,bytes,bytes[])

- **Signature**: `encodeFromBlueprint(bytes4,bytes,bytes[])`
- **Visibility**: public
- **Source Range**: 26673:1950:22
- **Details**: [function_encodeFromBlueprint_bytes4_bytes_bytes[].md](./function_encodeFromBlueprint_bytes4_bytes_bytes[].md)

**Signature:**
```solidity
///  @notice Encodes a full ABI payload, prepending a 4-byte function selector.
///  @param selector The function selector.
///  @param blueprint The byte-level layout description.
///  @param registers Caller-supplied data referenced by the blueprint.
///  @return out A (4 + N)-byte ABI blob suitable for a low-level `call`.
function encodeFromBlueprint(bytes4 selector, bytes calldata blueprint, bytes[] memory registers) public pure returns (bytes memory out);
```

### encodeData(bytes,bytes[])

- **Signature**: `encodeData(bytes,bytes[])`
- **Visibility**: public
- **Source Range**: 29064:1264:22
- **Details**: [function_encodeData_bytes_bytes[].md](./function_encodeData_bytes_bytes[].md)

**Signature:**
```solidity
///  @notice Encodes an ABI data blob *without* a function selector.
///  @param blueprint Layout description.
///  @param registers Caller-supplied registers.
///  @return out ABI-encoded data blob.
function encodeData(bytes calldata blueprint, bytes[] memory registers) public pure returns (bytes memory out);
```
