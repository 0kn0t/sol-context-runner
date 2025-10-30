# Function: fallback()

**Contract**: [src/proxy/MinimalProxy.sol/contract_MinimalProxy.md]

## Metadata

- **Contract**: MinimalProxy
- **Signature**: `fallback()`
- **Visibility**: external
- **Source Range**: 462:511:39

## Implementation

```solidity
fallback() external payable {
    if (msg.sender != owner) revert VmErrors.Unauthorized();
    (bool success, bytes memory returnData) = vmAddress.delegatecall(msg.data);
    assembly {
        switch success
        case 0 {
            revert(add(returnData, 0x20), mload(returnData))
        }
        default {
            return(add(returnData, 0x20), mload(returnData))
        }
    }
}
```

## External Calls

- **address::delegatecall(bytes calldata)**

## State Variable Reads

- **owner** (`address`)
- **vmAddress** (`address`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: MinimalProxy.fallback() (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: external
```
