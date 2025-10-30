# Function: setUserRole(address,uint8,bool)

**Contract**: [src/Dependencies/Governor.sol/contract_Governor.md]

## Metadata

- **Contract**: Governor
- **Signature**: `setUserRole(address,uint8,bool)`
- **Visibility**: public
- **Source Range**: 5911:543:37
- **Inherited From**: RolesAuthority

## Implementation

```solidity
function setUserRole(address user, uint8 role, bool enabled) virtual public requiresAuth() {
    if (enabled) {
        getUserRoles[user] |= bytes32(1 << role);
        if (!users.contains(user)) {
            users.add(user);
        }
    } else {
        getUserRoles[user] &= ~bytes32(1 << role);
        if (getUserRoles[user] == bytes32(0)) {
            users.remove(user);
        }
    }
    emit UserRoleUpdated(user, role, enabled);
}
```

## Related Implementations

### contains(struct EnumerableSet.AddressSet,address)

- **Kind**: internal
- **Source**: 8860:165:27
- **Link**: `src/Dependencies/EnumerableSet.sol:EnumerableSet:contains(struct EnumerableSet.AddressSet,address)`

```solidity
///  @dev Returns true if the value is in the set. O(1).
function contains(AddressSet storage set, address value) internal view returns (bool) {
    return _contains(set._inner, bytes32(uint256(uint160(value))));
}
```

### _contains(struct EnumerableSet.Set,bytes32)

- **Kind**: internal
- **Source**: 4255:127:27
- **Link**: `src/Dependencies/EnumerableSet.sol:EnumerableSet:_contains(struct EnumerableSet.Set,bytes32)`

```solidity
///  @dev Returns true if the value is in the set. O(1).
function _contains(Set storage set, bytes32 value) private view returns (bool) {
    return set._indexes[value] != 0;
}
```

### add(struct EnumerableSet.AddressSet,address)

- **Kind**: internal
- **Source**: 8305:150:27
- **Link**: `src/Dependencies/EnumerableSet.sol:EnumerableSet:add(struct EnumerableSet.AddressSet,address)`

```solidity
///  @dev Add a value to a set. O(1).
///  Returns true if the value was added to the set, that is if it was not
///  already present.
function add(AddressSet storage set, address value) internal returns (bool) {
    return _add(set._inner, bytes32(uint256(uint160(value))));
}
```

### _add(struct EnumerableSet.Set,bytes32)

- **Kind**: internal
- **Source**: 2214:404:27
- **Link**: `src/Dependencies/EnumerableSet.sol:EnumerableSet:_add(struct EnumerableSet.Set,bytes32)`

```solidity
///  @dev Add a value to a set. O(1).
///  Returns true if the value was added to the set, that is if it was not
///  already present.
function _add(Set storage set, bytes32 value) private returns (bool) {
    if (!_contains(set, value)) {
        set._values.push(value);
        set._indexes[value] = set._values.length;
        return true;
    } else {
        return false;
    }
}
```

### remove(struct EnumerableSet.AddressSet,address)

- **Kind**: internal
- **Source**: 8623:156:27
- **Link**: `src/Dependencies/EnumerableSet.sol:EnumerableSet:remove(struct EnumerableSet.AddressSet,address)`

```solidity
///  @dev Removes a value from a set. O(1).
///  Returns true if the value was removed from the set, that is if it was
///  present.
function remove(AddressSet storage set, address value) internal returns (bool) {
    return _remove(set._inner, bytes32(uint256(uint160(value))));
}
```

### _remove(struct EnumerableSet.Set,bytes32)

- **Kind**: internal
- **Source**: 2786:1388:27
- **Link**: `src/Dependencies/EnumerableSet.sol:EnumerableSet:_remove(struct EnumerableSet.Set,bytes32)`

```solidity
///  @dev Removes a value from a set. O(1).
///  Returns true if the value was removed from the set, that is if it was
///  present.
function _remove(Set storage set, bytes32 value) private returns (bool) {
    uint256 valueIndex = set._indexes[value];
    if (valueIndex != 0) {
        uint256 toDeleteIndex = valueIndex - 1;
        uint256 lastIndex = set._values.length - 1;
        if (lastIndex != toDeleteIndex) {
            bytes32 lastValue = set._values[lastIndex];
            set._values[toDeleteIndex] = lastValue;
            set._indexes[lastValue] = valueIndex;
        }
        set._values.pop();
        delete set._indexes[value];
        return true;
    } else {
        return false;
    }
}
```

### requiresAuth()

- **Kind**: modifier
- **Source**: 895:125:24
- **Link**: `src/Dependencies/Auth.sol:Auth:requiresAuth()`

```solidity
modifier requiresAuth() virtual {
    require(isAuthorized(msg.sender, msg.sig), "Auth: UNAUTHORIZED");
    _;
}
```

### isAuthorized(address,bytes4)

- **Kind**: internal
- **Source**: 1026:564:24
- **Link**: `src/Dependencies/Auth.sol:Auth:isAuthorized(address,bytes4)`

```solidity
function isAuthorized(address user, bytes4 functionSig) virtual internal view returns (bool) {
    Authority auth = authority;
    return ((address(auth) != address(0)) && auth.canCall(user, address(this), functionSig)) || (user == owner);
}
```

## State Variable Reads

- **users** (`struct EnumerableSet.AddressSet`)
- **getUserRoles** (`mapping(address => bytes32)`)
- **authority** (`contract Authority`) [src/Dependencies/Authority.sol/interface_Authority.md]
- **owner** (`address`)

## State Variable Writes

- **getUserRoles** (`mapping(address => bytes32)`)
- **users** (`struct EnumerableSet.AddressSet`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: RolesAuthority.setUserRole(address,uint8,bool) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
  ├─ [1] ⚙️ FUNCTION: EnumerableSet.contains(struct EnumerableSet.AddressSet,address) (NodeID: 1)
  │   💬 Args: [users, user]
  │   👁️  Def: internal
  │ └─ [2] ⚙️ FUNCTION: EnumerableSet._contains(struct EnumerableSet.Set,bytes32) (NodeID: 2)
  │     💬 Args: [set._inner, bytes32(uint256(uint160(value)))]
  │     👁️  Def: private
  ├─ [1] ⚙️ FUNCTION: EnumerableSet.add(struct EnumerableSet.AddressSet,address) (NodeID: 3)
  │   💬 Args: [users, user]
  │   👁️  Def: internal
  │ └─ [2] ⚙️ FUNCTION: EnumerableSet._add(struct EnumerableSet.Set,bytes32) (NodeID: 4)
  │     💬 Args: [set._inner, bytes32(uint256(uint160(value)))]
  │     👁️  Def: private
  │   └─ [3] ⚙️ FUNCTION: EnumerableSet._contains(struct EnumerableSet.Set,bytes32) (NodeID: 5)
  │       💬 Args: [set, value]
  │       👁️  Def: private
  ├─ [1] ⚙️ FUNCTION: EnumerableSet.remove(struct EnumerableSet.AddressSet,address) (NodeID: 6)
  │   💬 Args: [users, user]
  │   👁️  Def: internal
  │ └─ [2] ⚙️ FUNCTION: EnumerableSet._remove(struct EnumerableSet.Set,bytes32) (NodeID: 7)
  │     💬 Args: [set._inner, bytes32(uint256(uint160(value)))]
  │     👁️  Def: private
  └─ [1] 🔒 MODIFIER: Auth.requiresAuth() (NodeID: 8)
      💬 Args: [no args]
    └─ [2] ⚙️ FUNCTION: Auth.isAuthorized(address,bytes4) (NodeID: 9)
        💬 Args: [msg.sender, msg.sig]
        👁️  Def: internal
```
