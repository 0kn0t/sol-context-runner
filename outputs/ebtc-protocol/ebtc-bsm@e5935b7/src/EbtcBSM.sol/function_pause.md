# Function: pause()

**Contract**: [src/EbtcBSM.sol/contract_EbtcBSM.md]

## Metadata

- **Contract**: EbtcBSM
- **Signature**: `pause()`
- **Visibility**: external
- **Source Range**: 19866:64:40

## Implementation

```solidity
/// @notice Pauses the contract operations
///  @dev Can only be called by authorized users
function pause() external requiresAuth() {
    _pause();
}
```

## Related Implementations

### _pause()

- **Kind**: internal
- **Source**: 2463:115:15
- **Link**: `lib/openzeppelin-contracts/contracts/utils/Pausable.sol:Pausable:_pause()`

```solidity
///  @dev Triggers stopped state.
///  Requirements:
///  - The contract must not be paused.
function _pause() virtual internal whenNotPaused() {
    _paused = true;
    emit Paused(_msgSender());
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

### whenNotPaused()

- **Kind**: modifier
- **Source**: 1439:72:15
- **Link**: `lib/openzeppelin-contracts/contracts/utils/Pausable.sol:Pausable:whenNotPaused()`

```solidity
///  @dev Modifier to make a function callable only when the contract is not paused.
///  Requirements:
///  - The contract must not be paused.
modifier whenNotPaused() {
    _requireNotPaused();
    _;
}
```

### _requireNotPaused()

- **Kind**: internal
- **Source**: 2002:128:15
- **Link**: `lib/openzeppelin-contracts/contracts/utils/Pausable.sol:Pausable:_requireNotPaused()`

```solidity
///  @dev Throws if the contract is paused.
function _requireNotPaused() virtual internal view {
    if (paused()) {
        revert EnforcedPause();
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
┌─ [0] ⚙️ FUNCTION: EbtcBSM.pause() (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: external
  ├─ [1] ⚙️ FUNCTION: Pausable._pause() (NodeID: 1)
  │   💬 Args: [no args]
  │   👁️  Def: internal
  │ ├─ [2] ⚙️ FUNCTION: Context._msgSender() (NodeID: 2)
  │ │   💬 Args: [no args]
  │ │   👁️  Def: internal
  │ └─ [2] 🔒 MODIFIER: Pausable.whenNotPaused() (NodeID: 3)
  │     💬 Args: [no args]
  │   └─ [3] ⚙️ FUNCTION: Pausable._requireNotPaused() (NodeID: 4)
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

@notice Pauses the contract operations
 @dev Can only be called by authorized users
