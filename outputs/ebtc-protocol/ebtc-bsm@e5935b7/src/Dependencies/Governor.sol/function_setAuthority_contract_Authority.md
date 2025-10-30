# Function: setAuthority(contract Authority)

**Contract**: [src/Dependencies/Governor.sol/contract_Governor.md]

## Metadata

- **Contract**: Governor
- **Signature**: `setAuthority(contract Authority)`
- **Visibility**: public
- **Source Range**: 1596:434:24
- **Inherited From**: Auth

## Implementation

```solidity
function setAuthority(Authority newAuthority) virtual public {
    require((msg.sender == owner) || authority.canCall(msg.sender, address(this), msg.sig));
    authority = newAuthority;
    emit AuthorityUpdated(msg.sender, newAuthority);
}
```

## External Calls

- **Authority::canCall(address,address,bytes4)**

## State Variable Reads

- **owner** (`address`)
- **authority** (`contract Authority`) [src/Dependencies/Authority.sol/interface_Authority.md]

## State Variable Writes

- **authority** (`contract Authority`) [src/Dependencies/Authority.sol/interface_Authority.md]

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: Auth.setAuthority(contract Authority) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
```
