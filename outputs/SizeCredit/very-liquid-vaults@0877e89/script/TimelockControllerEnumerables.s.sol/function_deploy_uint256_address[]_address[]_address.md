# Function: deploy(uint256,address[],address[],address)

**Contract**: [script/TimelockControllerEnumerables.s.sol/contract_TimelockControllerEnumerablesScript.md]

## Metadata

- **Contract**: TimelockControllerEnumerablesScript
- **Signature**: `deploy(uint256,address[],address[],address)`
- **Visibility**: public
- **Source Range**: 2333:274:139

## Implementation

```solidity
function deploy(uint256 minDelay_, address[] memory proposers_, address[] memory executors_, address admin_) public returns (TimelockControllerEnumerable) {
    return new TimelockControllerEnumerable(minDelay_, proposers_, executors_, admin_);
}
```

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: TimelockControllerEnumerablesScript.deploy(uint256,address[],address[],address) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
```
