# Function: unpause()

**Contract**: [src/EbtcBSM.sol/contract_EbtcBSM.md]

## Metadata

- **Contract**: EbtcBSM
- **Signature**: `unpause()`
- **Visibility**: external
- **Source Range**: 20037:68:40

## Implementation

```solidity
/// @notice Unpauses the contract operations
///  @dev Can only be called by authorized users
function unpause() external requiresAuth() {
    _unpause();
}
```

## Related Implementations

### _unpause()

- **Kind**: internal
- **Source**: 2710:117:15
- **Link**: `lib/openzeppelin-contracts/contracts/utils/Pausable.sol:Pausable:_unpause()`

```solidity
///  @dev Returns to normal state.
///  Requirements:
///  - The contract must be paused.
function _unpause() virtual internal whenPaused() {
    _paused = false;
    emit Unpaused(_msgSender());
}
```

### _msgSender()

- **Kind**: internal
- **Source**: 656:96:12
- **Link**: `lib/openzeppelin-contracts/contracts/utils/Context.sol:Context:_msgSender()`

```solidity
function _msgSender() virtual internal view returns (address) {
    return msg.sender;
}
```

### whenPaused()

- **Kind**: modifier
- **Source**: 1689:66:15
- **Link**: `lib/openzeppelin-contracts/contracts/utils/Pausable.sol:Pausable:whenPaused()`

```solidity
///  @dev Modifier to make a function callable only when the contract is paused.
///  Requirements:
///  - The contract must be paused.
modifier whenPaused() {
    _requirePaused();
    _;
}
```

### _requirePaused()

- **Kind**: internal
- **Source**: 2202:126:15
- **Link**: `lib/openzeppelin-contracts/contracts/utils/Pausable.sol:Pausable:_requirePaused()`

```solidity
///  @dev Throws if the contract is not paused.
function _requirePaused() virtual internal view {
    if (!paused()) {
        revert ExpectedPause();
    }
}
```

### paused()

- **Kind**: internal
- **Source**: 1850:84:15
- **Link**: `lib/openzeppelin-contracts/contracts/utils/Pausable.sol:Pausable:paused()`

```solidity
///  @dev Returns true if the contract is paused, and false otherwise.
function paused() virtual public view returns (bool) {
    return _paused;
}
```

### requiresAuth()

- **Kind**: modifier
- **Source**: 647:125:25
- **Link**: `src/Dependencies/AuthNoOwner.sol:AuthNoOwner:requiresAuth()`

```solidity
modifier requiresAuth() virtual {
    require(isAuthorized(msg.sender, msg.sig), "Auth: UNAUTHORIZED");
    _;
}
```

### isAuthorized(address,bytes4)

- **Kind**: internal
- **Source**: 981:524:25
- **Link**: `src/Dependencies/AuthNoOwner.sol:AuthNoOwner:isAuthorized(address,bytes4)`

```solidity
function isAuthorized(address user, bytes4 functionSig) virtual internal view returns (bool) {
    Authority auth = _authority;
    return ((address(auth) != address(0)) && auth.canCall(user, address(this), functionSig));
}
```

## State Variable Reads

- **_paused** (`bool`)
- **_authority** (`contract Authority`) [src/Dependencies/Authority.sol/interface_Authority.md]

## State Variable Writes

- **_paused** (`bool`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: EbtcBSM.unpause() (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: external
  ├─ [1] ⚙️ FUNCTION: Pausable._unpause() (NodeID: 1)
  │   💬 Args: [no args]
  │   👁️  Def: internal
  │ ├─ [2] ⚙️ FUNCTION: Context._msgSender() (NodeID: 2)
  │ │   💬 Args: [no args]
  │ │   👁️  Def: internal
  │ └─ [2] 🔒 MODIFIER: Pausable.whenPaused() (NodeID: 3)
  │     💬 Args: [no args]
  │   └─ [3] ⚙️ FUNCTION: Pausable._requirePaused() (NodeID: 4)
  │       💬 Args: [no args]
  │       👁️  Def: internal
  │     └─ [4] ⚙️ FUNCTION: Pausable.paused() (NodeID: 5)
  │         💬 Args: [no args]
  │         👁️  Def: public
  └─ [1] 🔒 MODIFIER: AuthNoOwner.requiresAuth() (NodeID: 6)
      💬 Args: [no args]
    └─ [2] ⚙️ FUNCTION: AuthNoOwner.isAuthorized(address,bytes4) (NodeID: 7)
        💬 Args: [msg.sender, msg.sig]
        👁️  Def: internal
```

## Documentation

### Function Documentation

@notice Unpauses the contract operations
 @dev Can only be called by authorized users
