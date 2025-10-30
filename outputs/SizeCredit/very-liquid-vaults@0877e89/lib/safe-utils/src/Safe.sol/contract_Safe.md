# Contract: Safe

## Metadata

- **Name**: Safe
- **Type**: Contract
- **Path**: lib/safe-utils/src/Safe.sol

## State Variables

### vm

```solidity
Vm internal constant vm = Vm(address(bytes20(uint160(uint256(keccak256("hevm cheat code"))))))
```

**Vm**: [lib/forge-std/src/Vm.sol/interface_Vm.md]

### SAFE_THRESHOLD_STORAGE_SLOT

```solidity
bytes32 internal constant SAFE_THRESHOLD_STORAGE_SLOT = bytes32(uint256(4))
```

### MULTI_SEND_CALL_ONLY_ADDRESS_CANONICAL

```solidity
address internal constant MULTI_SEND_CALL_ONLY_ADDRESS_CANONICAL = 0x40A2aCCbd92BCA938b02010E17A5b8929b49130D
```

### MULTI_SEND_CALL_ONLY_ADDRESS_EIP155

```solidity
address internal constant MULTI_SEND_CALL_ONLY_ADDRESS_EIP155 = 0xA1dabEF33b3B82c7814B6D82A79e50F4AC44102B
```

### MULTI_SEND_CALL_ONLY_ADDRESS_ZKSYNC

```solidity
address internal constant MULTI_SEND_CALL_ONLY_ADDRESS_ZKSYNC = 0xf220D3b4DFb23C4ade8C88E526C1353AbAcbC38F
```

## Structs

### Instance

```solidity
struct Instance {
    address safe;
    HTTP.Client http;
    mapping(uint256 => string) urls;
    mapping(uint256 => MultiSendCallOnly) multiSendCallOnly;
    string requestBody;
}
```

### Signature

```solidity
struct Signature {
    uint8 v;
    bytes32 r;
    bytes32 s;
}
```

### Client

```solidity
struct Client {
    Instance[] instances;
}
```

### ExecTransactionParams

```solidity
struct ExecTransactionParams {
    address to;
    uint256 value;
    bytes data;
    Enum.Operation operation;
    address sender;
    bytes signature;
    uint256 nonce;
}
```

## Errors

### ApiKitUrlNotFound

```solidity
error ApiKitUrlNotFound(uint256 chainId);
```

### MultiSendCallOnlyNotFound

```solidity
error MultiSendCallOnlyNotFound(uint256 chainId);
```

### ArrayLengthsMismatch

```solidity
error ArrayLengthsMismatch(uint256 a, uint256 b);
```

### ProposeTransactionFailed

```solidity
error ProposeTransactionFailed(uint256 statusCode, string response);
```
