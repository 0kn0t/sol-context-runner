# Function: updateDelay(uint256)

**Contract**: [lib/openzeppelin-community-contracts/contracts/governance/TimelockControllerEnumerable.sol/contract_TimelockControllerEnumerable.md]

## Metadata

- **Contract**: TimelockControllerEnumerable
- **Signature**: `updateDelay(uint256)`
- **Visibility**: external
- **Source Range**: 15493:286:72
- **Inherited From**: TimelockController

## Implementation

```solidity
///  @dev Changes the minimum timelock duration for future operations.
///  Emits a {MinDelayChange} event.
///  Requirements:
///  - the caller must be the timelock itself. This can only be achieved by scheduling and later executing
///  an operation where the timelock is the target and the data is the ABI-encoded call to this function.
function updateDelay(uint256 newDelay) virtual external {
    address sender = _msgSender();
    if (sender != address(this)) {
        revert TimelockUnauthorizedCaller(sender);
    }
    emit MinDelayChange(_minDelay, newDelay);
    _minDelay = newDelay;
}
```

## Related Implementations

### _msgSender()

- **Kind**: internal
- **Source**: 656:96:98
- **Link**: `lib/openzeppelin-contracts/contracts/utils/Context.sol:Context:_msgSender()`

```solidity
function _msgSender() virtual internal view returns (address) {
    return msg.sender;
}
```

## State Variable Reads

- **_minDelay** (`uint256`)

## State Variable Writes

- **_minDelay** (`uint256`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: TimelockController.updateDelay(uint256) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: external
  └─ [1] ⚙️ FUNCTION: Context._msgSender() (NodeID: 1)
      💬 Args: [no args]
      👁️  Def: internal
```

## Documentation

### Function Documentation

 @dev Changes the minimum timelock duration for future operations.
 Emits a {MinDelayChange} event.
 Requirements:
 - the caller must be the timelock itself. This can only be achieved by scheduling and later executing
 an operation where the timelock is the target and the data is the ABI-encoded call to this function.
