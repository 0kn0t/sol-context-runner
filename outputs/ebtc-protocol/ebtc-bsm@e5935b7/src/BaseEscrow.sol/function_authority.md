# Function: authority()

**Contract**: [src/BaseEscrow.sol/contract_BaseEscrow.md]

## Metadata

- **Contract**: BaseEscrow
- **Signature**: `authority()`
- **Visibility**: public
- **Source Range**: 778:87:25
- **Inherited From**: AuthNoOwner

## Implementation

```solidity
function authority() public view returns (Authority) {
    return _authority;
}
```

## State Variable Reads

- **_authority** (`contract Authority`) [src/Dependencies/Authority.sol/interface_Authority.md]

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: AuthNoOwner.authority() (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
```
