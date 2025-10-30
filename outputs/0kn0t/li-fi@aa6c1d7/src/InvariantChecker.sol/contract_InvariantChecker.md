# Contract: InvariantChecker

## Metadata

- **Name**: InvariantChecker
- **Type**: Contract
- **Path**: src/InvariantChecker.sol
- **Documentation**:  @title InvariantChecker
   @notice Companion contract for VM runtime invariant checking
   @dev Gas-optimized contract that performs basic register value comparisons

## Errors

### AssertEqFailed

```solidity
error AssertEqFailed(uint256 a, uint256 b);
```

### AssertNeqFailed

```solidity
error AssertNeqFailed(uint256 a, uint256 b);
```

### AssertLtFailed

```solidity
error AssertLtFailed(uint256 a, uint256 b);
```

### AssertGtFailed

```solidity
error AssertGtFailed(uint256 a, uint256 b);
```

### AssertLteFailed

```solidity
error AssertLteFailed(uint256 a, uint256 b);
```

### AssertGteFailed

```solidity
error AssertGteFailed(uint256 a, uint256 b);
```

### AssertRangeFailed

```solidity
error AssertRangeFailed(uint256 value, uint256 min, uint256 max);
```

### UnknownOpcode

```solidity
error UnknownOpcode(uint8 opcode);
```

## Public/External Functions

### assertEqual(uint256,uint256)

- **Signature**: `assertEqual(uint256,uint256)`
- **Visibility**: external
- **Source Range**: 787:113:27
- **Details**: [function_assertEqual_uint256_uint256.md](./function_assertEqual_uint256_uint256.md)

**Signature:**
```solidity
///  @notice Asserts that two values are equal
function assertEqual(uint256 a, uint256 b) external pure;
```

### assertNotEqual(uint256,uint256)

- **Signature**: `assertNotEqual(uint256,uint256)`
- **Visibility**: external
- **Source Range**: 975:117:27
- **Details**: [function_assertNotEqual_uint256_uint256.md](./function_assertNotEqual_uint256_uint256.md)

**Signature:**
```solidity
///  @notice Asserts that two values are not equal
function assertNotEqual(uint256 a, uint256 b) external pure;
```

### assertLessThan(uint256,uint256)

- **Signature**: `assertLessThan(uint256,uint256)`
- **Visibility**: external
- **Source Range**: 1182:116:27
- **Details**: [function_assertLessThan_uint256_uint256.md](./function_assertLessThan_uint256_uint256.md)

**Signature:**
```solidity
///  @notice Asserts that the first value is less than the second
function assertLessThan(uint256 a, uint256 b) external pure;
```

### assertGreaterThan(uint256,uint256)

- **Signature**: `assertGreaterThan(uint256,uint256)`
- **Visibility**: external
- **Source Range**: 1391:119:27
- **Details**: [function_assertGreaterThan_uint256_uint256.md](./function_assertGreaterThan_uint256_uint256.md)

**Signature:**
```solidity
///  @notice Asserts that the first value is greater than the second
function assertGreaterThan(uint256 a, uint256 b) external pure;
```

### assertLessThanEqual(uint256,uint256)

- **Signature**: `assertLessThanEqual(uint256,uint256)`
- **Visibility**: external
- **Source Range**: 1612:121:27
- **Details**: [function_assertLessThanEqual_uint256_uint256.md](./function_assertLessThanEqual_uint256_uint256.md)

**Signature:**
```solidity
///  @notice Asserts that the first value is less than or equal to the second
function assertLessThanEqual(uint256 a, uint256 b) external pure;
```

### assertGreaterThanEqual(uint256,uint256)

- **Signature**: `assertGreaterThanEqual(uint256,uint256)`
- **Visibility**: external
- **Source Range**: 1838:124:27
- **Details**: [function_assertGreaterThanEqual_uint256_uint256.md](./function_assertGreaterThanEqual_uint256_uint256.md)

**Signature:**
```solidity
///  @notice Asserts that the first value is greater than or equal to the second
function assertGreaterThanEqual(uint256 a, uint256 b) external pure;
```

### assertInRange(uint256,uint256,uint256)

- **Signature**: `assertInRange(uint256,uint256,uint256)`
- **Visibility**: external
- **Source Range**: 2200:168:27
- **Details**: [function_assertInRange_uint256_uint256_uint256.md](./function_assertInRange_uint256_uint256_uint256.md)

**Signature:**
```solidity
///  @notice Asserts that a value is within an inclusive range
///  @param value The value to check
///  @param min The minimum allowed value (inclusive)
///  @param max The maximum allowed value (inclusive)
function assertInRange(uint256 value, uint256 min, uint256 max) external pure;
```

### batchAssert(uint8[],uint256[])

- **Signature**: `batchAssert(uint8[],uint256[])`
- **Visibility**: external
- **Source Range**: 5339:336:27
- **Details**: [function_batchAssert_uint8[]_uint256[].md](./function_batchAssert_uint8[]_uint256[].md)

**Signature:**
```solidity
///  @notice Batch check multiple assertions to save gas on multiple comparisons
///  @param ops Array of operation codes (0=nop, 1=eq, 2=neq, 3=lt, 4=gt, 5=lte, 6=gte, 7=range)
///  @param values Array of values to check (for range check: [value, min, max])
///  @dev Op 0 (nop) performs no operation and doesn't consume any values
///  @dev For ops 1-6: values[valueIdx] and values[valueIdx+1] are compared
///  @dev For op 7: values[valueIdx], values[valueIdx+1], and values[valueIdx+2] are used
///  @dev Note: valueIdx is dynamically updated based on the operations executed
function batchAssert(uint8[] calldata ops, uint256[] calldata values) external pure;
```

### batchAssertPacked(uint256,uint8,uint256[])

- **Signature**: `batchAssertPacked(uint256,uint8,uint256[])`
- **Visibility**: external
- **Source Range**: 6403:435:27
- **Details**: [function_batchAssertPacked_uint256_uint8_uint256[].md](./function_batchAssertPacked_uint256_uint8_uint256[].md)

**Signature:**
```solidity
///  @notice Gas-optimized batch check using packed opcodes in a single uint256
///  @param packedOps A uint256 with packed operation codes, each taking 8 bits (0=nop, 1=eq, 2=neq, 3=lt, 4=gt, 5=lte, 6=gte, 7=range)
///  @param opCount Number of operations packed into packedOps (max 32)
///  @param values Array of values to check (for range check: [value, min, max])
///  @dev Op 0 (nop) performs no operation and doesn't consume any values
///  @dev For ops 1-6: values[valueIdx] and values[valueIdx+1] are compared
///  @dev For op 7: values[valueIdx], values[valueIdx+1], and values[valueIdx+2] are used
///  @dev Note: valueIdx is dynamically updated based on the operations executed
function batchAssertPacked(uint256 packedOps, uint8 opCount, uint256[] calldata values) external pure;
```

### packOps(uint8[])

- **Signature**: `packOps(uint8[])`
- **Visibility**: external
- **Source Range**: 7117:373:27
- **Details**: [function_packOps_uint8[].md](./function_packOps_uint8[].md)

**Signature:**
```solidity
///  @notice Helper function to pack operation codes into a single uint256
///  @param ops Array of operation codes to pack
///  @return packed The packed uint256 with all operation codes
///  @dev This is a pure view function primarily for testing
function packOps(uint8[] memory ops) external pure returns (uint256 packed);
```
