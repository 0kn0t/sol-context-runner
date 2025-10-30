# Function: constructor(address)

**Contract**: [src/Dependencies/Governor.sol/contract_Governor.md]

## Metadata

- **Contract**: Governor
- **Signature**: `constructor(address)`
- **Visibility**: public
- **Source Range**: 1350:79:28

## Implementation

```solidity
/// @notice The contract constructor initializes RolesAuthority with the given owner.
///  @param _owner The address of the owner, who gains all permissions by default.
constructor(address _owner) RolesAuthority(_owner,Authority(address(this))) {}
```

## Related Implementations

### (address,contract Authority)

- **Kind**: internal
- **Source**: 867:77:37
- **Link**: `src/Dependencies/RolesAuthority.sol:RolesAuthority:constructor(address,contract Authority)`

```solidity
constructor(address _owner, Authority _authority) Auth(_owner,_authority) {}
```

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
┌─ [0] 🏗️ CONSTRUCTOR: Governor.constructor(address) (NodeID: 0)
    💬 Args: [no args]
    🏗️  Contract: Governor
  └─ [1] 🏗️ CONSTRUCTOR: RolesAuthority.constructor(address,contract Authority) (NodeID: 1)
      💬 Args: [_owner, Authority(address(this))]
      🏗️  Contract: RolesAuthority
    └─ [2] 🏗️ CONSTRUCTOR: Auth.constructor(address,contract Authority) (NodeID: 2)
        💬 Args: [_owner, Authority(address(this))]
        🏗️  Contract: Auth
```

## Documentation

### Function Documentation

@notice The contract constructor initializes RolesAuthority with the given owner.
 @param _owner The address of the owner, who gains all permissions by default.
