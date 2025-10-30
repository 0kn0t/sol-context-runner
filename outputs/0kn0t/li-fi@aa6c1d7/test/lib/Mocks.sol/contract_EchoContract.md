# Contract: EchoContract

## Metadata

- **Name**: EchoContract
- **Type**: Contract
- **Path**: test/lib/Mocks.sol
- **Documentation**: @notice Consolidated contract for all echo functionality.
   @dev Example: EchoContract echo = new EchoContract();

## Structs

### Tuple

```solidity
struct Tuple {
    string a;
    string b;
}
```

## Public/External Functions

### fallback(bytes)

- **Signature**: `fallback(bytes)`
- **Visibility**: external
- **Source Range**: 441:97:41
- **Details**: [function_fallback_bytes.md](./function_fallback_bytes.md)

**Signature:**
```solidity
/// @notice Return exact calldata received.
///  @dev Example: bytes memory result = echo.echo(data);
fallback(bytes calldata) external payable returns (bytes memory);
```

### receive()

- **Signature**: `receive()`
- **Visibility**: external
- **Source Range**: 596:30:41
- **Details**: [function_receive.md](./function_receive.md)

**Signature:**
```solidity
/// @notice Receive function for ETH transfers.
receive() external payable;
```

### echo(string)

- **Signature**: `echo(string)`
- **Visibility**: external
- **Source Range**: 743:102:41
- **Details**: [function_echo_string.md](./function_echo_string.md)

**Signature:**
```solidity
/// @notice Return exact string received.
///  @dev Example: string memory result = echo.echo("hello");
function echo(string memory input) external pure returns (string memory);
```

### echo(bytes)

- **Signature**: `echo(bytes)`
- **Visibility**: external
- **Source Range**: 957:100:41
- **Details**: [function_echo_bytes.md](./function_echo_bytes.md)

**Signature:**
```solidity
/// @notice Return exact bytes received.
///  @dev Example: bytes memory result = echo.echo(data);
function echo(bytes memory input) external pure returns (bytes memory);
```

### echo(uint256,string[],uint256)

- **Signature**: `echo(uint256,string[],uint256)`
- **Visibility**: external
- **Source Range**: 1211:236:41
- **Details**: [function_echo_uint256_string[]_uint256.md](./function_echo_uint256_string[]_uint256.md)

**Signature:**
```solidity
/// @notice Return exact tuple received.
///  @dev Example: (uint256, string[], uint256) memory result = echo.echo(value1, strings, value2);
function echo(uint256 value1, string[] memory strings, uint256 value2) external pure returns (uint256, string[] memory, uint256);
```

### echo(uint256[])

- **Signature**: `echo(uint256[])`
- **Visibility**: external
- **Source Range**: 1573:110:41
- **Details**: [function_echo_uint256[].md](./function_echo_uint256[].md)

**Signature:**
```solidity
/// @notice Return exact uint256 array received.
///  @dev Example: uint256[] memory result = echo.echo(values);
function echo(uint256[] memory values) external pure returns (uint256[] memory);
```

### echoUint256(uint256)

- **Signature**: `echoUint256(uint256)`
- **Visibility**: public
- **Source Range**: 1799:95:41
- **Details**: [function_echoUint256_uint256.md](./function_echoUint256_uint256.md)

**Signature:**
```solidity
/// @notice Return exact uint256 received.
///  @dev Example: uint256 result = target.echoUint256(42);
function echoUint256(uint256 value) public pure returns (uint256);
```

### echoBytes(bytes)

- **Signature**: `echoBytes(bytes)`
- **Visibility**: public
- **Source Range**: 2013:101:41
- **Details**: [function_echoBytes_bytes.md](./function_echoBytes_bytes.md)

**Signature:**
```solidity
/// @notice Return exact bytes received.
///  @dev Example: bytes memory result = target.echoBytes(data);
function echoBytes(bytes memory data) public pure returns (bytes memory);
```

### echoString(string)

- **Signature**: `echoString(string)`
- **Visibility**: public
- **Source Range**: 2239:98:41
- **Details**: [function_echoString_string.md](./function_echoString_string.md)

**Signature:**
```solidity
/// @notice Return exact string received.
///  @dev Example: string memory result = target.echoString("hello");
function echoString(string memory s) public pure returns (string memory);
```

### echoStrings(string,string)

- **Signature**: `echoStrings(string,string)`
- **Visibility**: public
- **Source Range**: 2474:138:41
- **Details**: [function_echoStrings_string_string.md](./function_echoStrings_string_string.md)

**Signature:**
```solidity
/// @notice Return two strings unchanged.
///  @dev Example: (string memory, string memory) = target.echoStrings("a", "b");
function echoStrings(string memory s, string memory s2) public pure returns (string memory, string memory);
```

### echoMixed(uint256,string)

- **Signature**: `echoMixed(uint256,string)`
- **Visibility**: public
- **Source Range**: 2744:122:41
- **Details**: [function_echoMixed_uint256_string.md](./function_echoMixed_uint256_string.md)

**Signature:**
```solidity
/// @notice Return mixed types unchanged.
///  @dev Example: (uint256, string memory) = target.echoMixed(42, "hello");
function echoMixed(uint256 x, string memory s) public pure returns (uint256, string memory);
```

### echoUint256Array(uint256[])

- **Signature**: `echoUint256Array(uint256[])`
- **Visibility**: public
- **Source Range**: 3001:114:41
- **Details**: [function_echoUint256Array_uint256[].md](./function_echoUint256Array_uint256[].md)

**Signature:**
```solidity
/// @notice Return uint256 array unchanged.
///  @dev Example: uint256[] memory result = target.echoUint256Array(values);
function echoUint256Array(uint256[] memory arr) public pure returns (uint256[] memory);
```

### echoStringArray(string[])

- **Signature**: `echoStringArray(string[])`
- **Visibility**: public
- **Source Range**: 3248:111:41
- **Details**: [function_echoStringArray_string[].md](./function_echoStringArray_string[].md)

**Signature:**
```solidity
/// @notice Return string array unchanged.
///  @dev Example: string[] memory result = target.echoStringArray(strings);
function echoStringArray(string[] memory arr) public pure returns (string[] memory);
```

### echoTuple(struct EchoContract.Tuple)

- **Signature**: `echoTuple(struct EchoContract.Tuple)`
- **Visibility**: external
- **Source Range**: 3485:103:41
- **Details**: [function_echoTuple_struct_EchoContract.Tuple.md](./function_echoTuple_struct_EchoContract.Tuple.md)

**Signature:**
```solidity
/// @notice Return tuple unchanged.
///  @dev Example: EchoContract.Tuple memory result = target.echoTuple(tup);
function echoTuple(Tuple calldata tup) external pure returns (Tuple memory);
```

### echoTwoTuples(struct EchoContract.Tuple,struct EchoContract.Tuple)

- **Signature**: `echoTwoTuples(struct EchoContract.Tuple,struct EchoContract.Tuple)`
- **Visibility**: external
- **Source Range**: 3726:202:41
- **Details**: [function_echoTwoTuples_struct_EchoContract.Tuple_struct_EchoContract.Tuple.md](./function_echoTwoTuples_struct_EchoContract.Tuple_struct_EchoContract.Tuple.md)

**Signature:**
```solidity
/// @notice Return two tuples unchanged.
///  @dev Example: (Tuple memory, Tuple memory) = target.echoTwoTuples(tup1, tup2);
function echoTwoTuples(Tuple calldata tup1, Tuple calldata tup2) external pure returns (Tuple memory, Tuple memory);
```
