# Function: paused()

**Contract**: [src/VeryLiquidVault.sol/contract_VeryLiquidVault.md]

## Metadata

- **Contract**: VeryLiquidVault
- **Signature**: `paused()`
- **Visibility**: public
- **Source Range**: 2496:145:64
- **Inherited From**: PausableUpgradeable

## Implementation

```solidity
///  @dev Returns true if the contract is paused, and false otherwise.
function paused() virtual public view returns (bool) {
    PausableStorage storage $ = _getPausableStorage();
    return $._paused;
}
```

## Related Implementations

### _getPausableStorage()

- **Kind**: internal
- **Source**: 1147:162:64
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/utils/PausableUpgradeable.sol:PausableUpgradeable:_getPausableStorage()`

```solidity
function _getPausableStorage() private pure returns (PausableStorage storage $) {
    assembly {
        $.slot := PausableStorageLocation
    }
}
```

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: PausableUpgradeable.paused() (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
  └─ [1] ⚙️ FUNCTION: PausableUpgradeable._getPausableStorage() (NodeID: 1)
      💬 Args: [no args]
      👁️  Def: private
```

## Documentation

### Function Documentation

 @dev Returns true if the contract is paused, and false otherwise.
