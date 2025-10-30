# Function: constructor(address,address)

**Contract**: [src/proxy/MinimalProxy.sol/contract_MinimalProxy.md]

## Metadata

- **Contract**: MinimalProxy
- **Signature**: `constructor(address,address)`
- **Visibility**: public
- **Source Range**: 345:111:39

## Implementation

```solidity
constructor(address _owner, address _vmAddress) {
    owner = _owner;
    vmAddress = _vmAddress;
}
```

## State Variable Writes

- **owner** (`address`)
- **vmAddress** (`address`)

## Call Tree

```
┌─ [0] 🏗️ CONSTRUCTOR: MinimalProxy.constructor(address,address) (NodeID: 0)
    💬 Args: [no args]
    🏗️  Contract: MinimalProxy
```
