# Contract: HTTP

## Metadata

- **Name**: HTTP
- **Type**: Contract
- **Path**: lib/safe-utils/lib/solidity-http/src/HTTP.sol

## State Variables

### vm

```solidity
Vm internal constant vm = Vm(address(bytes20(uint160(uint256(keccak256("hevm cheat code"))))))
```

**Vm**: [lib/forge-std/src/Vm.sol/interface_Vm.md]

## Structs

### Request

```solidity
struct Request {
    string url;
    string body;
    Method method;
    StringMap.StringToStringMap headers;
    StringMap.StringToStringMap query;
}
```

### Response

```solidity
struct Response {
    uint256 status;
    string data;
}
```

### Client

```solidity
struct Client {
    Request[] requests;
}
```

## Errors

### HTTPArrayLengthsMismatch

```solidity
error HTTPArrayLengthsMismatch(uint256 a, uint256 b);
```

## Enums

### Method

```solidity
enum Method {
    GET,
    POST,
    PUT,
    DELETE,
    PATCH
}
```
