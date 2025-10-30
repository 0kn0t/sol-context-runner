# Function: constructor(address,contract Authority)

**Contract**: [src/Dependencies/RolesAuthority.sol/contract_RolesAuthority.md]

## Metadata

- **Contract**: RolesAuthority
- **Signature**: `constructor(address,contract Authority)`
- **Visibility**: public
- **Source Range**: 867:77:37

## Implementation

```solidity
constructor(address _owner, Authority _authority) Auth(_owner,_authority) {}
```

## Related Implementations

### (address,contract Authority)

- **Kind**: internal
- **Source**: 665:224:24
- **Link**: `src/Dependencies/Auth.sol:Auth:constructor(address,contract Authority)`

```solidity
constructor(address _owner, Authority _authority) {
    owner = _owner;
    authority = _authority;
    emit OwnershipTransferred(msg.sender, _owner);
    emit AuthorityUpdated(msg.sender, _authority);
}
```

## State Variable Writes

- **owner** (`address`)
- **authority** (`contract Authority`) [src/Dependencies/Authority.sol/interface_Authority.md]

## Call Tree

```
┌─ [0] 🏗️ CONSTRUCTOR: RolesAuthority.constructor(address,contract Authority) (NodeID: 0)
    💬 Args: [no args]
    🏗️  Contract: RolesAuthority
  └─ [1] 🏗️ CONSTRUCTOR: Auth.constructor(address,contract Authority) (NodeID: 1)
      💬 Args: [_owner, _authority]
      🏗️  Contract: Auth
```
