# Contract: MinimalProxy

## Metadata

- **Name**: MinimalProxy
- **Type**: Contract
- **Path**: src/proxy/MinimalProxy.sol
- **Documentation**:  @title Minimal Proxy
   @dev A minimal proxy that authenticates the sender and forwards all calls to the VM address

## State Variables

### owner

```solidity
address public immutable owner
```

### vmAddress

```solidity
address public immutable vmAddress
```

## Public/External Functions

### constructor(address,address)

- **Signature**: `constructor(address,address)`
- **Visibility**: public
- **Source Range**: 345:111:39
- **Details**: [function_constructor_address_address.md](./function_constructor_address_address.md)

**Signature:**
```solidity
constructor(address _owner, address _vmAddress);
```

### fallback()

- **Signature**: `fallback()`
- **Visibility**: external
- **Source Range**: 462:511:39
- **Details**: [function_fallback.md](./function_fallback.md)

**Signature:**
```solidity
fallback() external payable;
```

### receive()

- **Signature**: `receive()`
- **Visibility**: external
- **Source Range**: 979:30:39
- **Details**: [function_receive.md](./function_receive.md)

**Signature:**
```solidity
receive() external payable;
```
