# Function: constructor(address)

**Contract**: [src/proxy/ProxyFactory.sol/contract_ProxyFactory.md]

## Metadata

- **Contract**: ProxyFactory
- **Signature**: `constructor(address)`
- **Visibility**: public
- **Source Range**: 444:74:40

## Implementation

```solidity
constructor(address _vmContract) {
    vmContract = _vmContract;
}
```

## State Variable Writes

- **vmContract** (`address`)

## Call Tree

```
┌─ [0] 🏗️ CONSTRUCTOR: ProxyFactory.constructor(address) (NodeID: 0)
    💬 Args: [no args]
    🏗️  Contract: ProxyFactory
```
