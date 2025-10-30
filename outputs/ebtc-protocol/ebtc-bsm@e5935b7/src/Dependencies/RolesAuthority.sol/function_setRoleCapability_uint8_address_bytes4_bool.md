# Function: setRoleCapability(uint8,address,bytes4,bool)

**Contract**: [src/Dependencies/RolesAuthority.sol/contract_RolesAuthority.md]

## Metadata

- **Contract**: RolesAuthority
- **Signature**: `setRoleCapability(uint8,address,bytes4,bool)`
- **Visibility**: public
- **Source Range**: 4199:1083:37

## Implementation

```solidity
/// @notice Grant a specified role the ability to call a function on a target.
///  @notice Has no effect
function setRoleCapability(uint8 role, address target, bytes4 functionSig, bool enabled) virtual public requiresAuth() {
    if (enabled) {
        getRolesWithCapability[target][functionSig] |= bytes32(1 << role);
        enabledFunctionSigsByTarget[target].add(bytes32(functionSig));
        if (!targets.contains(target)) {
            targets.add(target);
        }
    } else {
        getRolesWithCapability[target][functionSig] &= ~bytes32(1 << role);
        if (getRolesWithCapability[target][functionSig] == bytes32(0)) {
            enabledFunctionSigsByTarget[target].remove(bytes32(functionSig));
        }
        if (enabledFunctionSigsByTarget[target].length() == 0) {
            targets.remove(target);
        }
    }
    emit RoleCapabilityUpdated(role, target, functionSig, enabled);
}
```

## Related Implementations

### add(struct EnumerableSet.Bytes32Set,bytes32)

- **Kind**: internal
- **Source**: 5919:123:27
- **Link**: `src/Dependencies/EnumerableSet.sol:EnumerableSet:add(struct EnumerableSet.Bytes32Set,bytes32)`

```solidity
///  @dev Add a value to a set. O(1).
///  Returns true if the value was added to the set, that is if it was not
///  already present.
function add(Bytes32Set storage set, bytes32 value) internal returns (bool) {
    return _add(set._inner, value);
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

### remove(struct EnumerableSet.Bytes32Set,bytes32)

- **Kind**: internal
- **Source**: 6210:129:27
- **Link**: `src/Dependencies/EnumerableSet.sol:EnumerableSet:remove(struct EnumerableSet.Bytes32Set,bytes32)`

```solidity
///  @dev Removes a value from a set. O(1).
///  Returns true if the value was removed from the set, that is if it was
///  present.
function remove(Bytes32Set storage set, bytes32 value) internal returns (bool) {
    return _remove(set._inner, value);
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

### length(struct EnumerableSet.Bytes32Set)

- **Kind**: internal
- **Source**: 6639:115:27
- **Link**: `src/Dependencies/EnumerableSet.sol:EnumerableSet:length(struct EnumerableSet.Bytes32Set)`

```solidity
///  @dev Returns the number of values in the set. O(1).
function length(Bytes32Set storage set) internal view returns (uint256) {
    return _length(set._inner);
}
```

### _length(struct EnumerableSet.Set)

- **Kind**: internal
- **Source**: 4463:107:27
- **Link**: `src/Dependencies/EnumerableSet.sol:EnumerableSet:_length(struct EnumerableSet.Set)`

```solidity
///  @dev Returns the number of values on the set. O(1).
function _length(Set storage set) private view returns (uint256) {
    return set._values.length;
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

- **targets** (`struct EnumerableSet.AddressSet`)
- **getRolesWithCapability** (`mapping(address => mapping(bytes4 => bytes32))`)
- **enabledFunctionSigsByTarget** (`mapping(address => struct EnumerableSet.Bytes32Set)`)
- **authority** (`contract Authority`) [src/Dependencies/Authority.sol/interface_Authority.md]
- **owner** (`address`)

## State Variable Writes

- **getRolesWithCapability** (`mapping(address => mapping(bytes4 => bytes32))`)
- **enabledFunctionSigsByTarget** (`mapping(address => struct EnumerableSet.Bytes32Set)`)
- **targets** (`struct EnumerableSet.AddressSet`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: RolesAuthority.setRoleCapability(uint8,address,bytes4,bool) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
  ├─ [1] ⚙️ FUNCTION: EnumerableSet.add(struct EnumerableSet.Bytes32Set,bytes32) (NodeID: 1)
  │   💬 Args: [enabledFunctionSigsByTarget[target], bytes32(functionSig)]
  │   👁️  Def: internal
  │ └─ [2] ⚙️ FUNCTION: EnumerableSet._add(struct EnumerableSet.Set,bytes32) (NodeID: 2)
  │     💬 Args: [set._inner, value]
  │     👁️  Def: private
  │   └─ [3] ⚙️ FUNCTION: EnumerableSet._contains(struct EnumerableSet.Set,bytes32) (NodeID: 3)
  │       💬 Args: [set, value]
  │       👁️  Def: private
  ├─ [1] ⚙️ FUNCTION: EnumerableSet.contains(struct EnumerableSet.AddressSet,address) (NodeID: 4)
  │   💬 Args: [targets, target]
  │   👁️  Def: internal
  │ └─ [2] ⚙️ FUNCTION: EnumerableSet._contains(struct EnumerableSet.Set,bytes32) (NodeID: 5)
  │     💬 Args: [set._inner, bytes32(uint256(uint160(value)))]
  │     👁️  Def: private
  ├─ [1] ⚙️ FUNCTION: EnumerableSet.add(struct EnumerableSet.AddressSet,address) (NodeID: 6)
  │   💬 Args: [targets, target]
  │   👁️  Def: internal
  │ └─ [2] ⚙️ FUNCTION: EnumerableSet._add(struct EnumerableSet.Set,bytes32) (NodeID: 7)
  │     💬 Args: [set._inner, bytes32(uint256(uint160(value)))]
  │     👁️  Def: private
  │   └─ [3] ⚙️ FUNCTION: EnumerableSet._contains(struct EnumerableSet.Set,bytes32) (NodeID: 8)
  │       💬 Args: [set, value]
  │       👁️  Def: private
  ├─ [1] ⚙️ FUNCTION: EnumerableSet.remove(struct EnumerableSet.Bytes32Set,bytes32) (NodeID: 9)
  │   💬 Args: [enabledFunctionSigsByTarget[target], bytes32(functionSig)]
  │   👁️  Def: internal
  │ └─ [2] ⚙️ FUNCTION: EnumerableSet._remove(struct EnumerableSet.Set,bytes32) (NodeID: 10)
  │     💬 Args: [set._inner, value]
  │     👁️  Def: private
  ├─ [1] ⚙️ FUNCTION: EnumerableSet.length(struct EnumerableSet.Bytes32Set) (NodeID: 11)
  │   💬 Args: [enabledFunctionSigsByTarget[target]]
  │   👁️  Def: internal
  │ └─ [2] ⚙️ FUNCTION: EnumerableSet._length(struct EnumerableSet.Set) (NodeID: 12)
  │     💬 Args: [set._inner]
  │     👁️  Def: private
  ├─ [1] ⚙️ FUNCTION: EnumerableSet.remove(struct EnumerableSet.AddressSet,address) (NodeID: 13)
  │   💬 Args: [targets, target]
  │   👁️  Def: internal
  │ └─ [2] ⚙️ FUNCTION: EnumerableSet._remove(struct EnumerableSet.Set,bytes32) (NodeID: 14)
  │     💬 Args: [set._inner, bytes32(uint256(uint160(value)))]
  │     👁️  Def: private
  └─ [1] 🔒 MODIFIER: Auth.requiresAuth() (NodeID: 15)
      💬 Args: [no args]
    └─ [2] ⚙️ FUNCTION: Auth.isAuthorized(address,bytes4) (NodeID: 16)
        💬 Args: [msg.sender, msg.sig]
        👁️  Def: internal
```

## Documentation

### Function Documentation

@notice Grant a specified role the ability to call a function on a target.
 @notice Has no effect
