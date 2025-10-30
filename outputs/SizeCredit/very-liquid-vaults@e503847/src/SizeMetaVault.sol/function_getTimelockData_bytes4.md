# Function: getTimelockData(bytes4)

**Contract**: [src/SizeMetaVault.sol/contract_SizeMetaVault.md]

## Metadata

- **Contract**: SizeMetaVault
- **Signature**: `getTimelockData(bytes4)`
- **Visibility**: public
- **Source Range**: 2180:120:64
- **Inherited From**: Timelock

## Implementation

```solidity
/// @notice Gets the timelock data for a specific function
function getTimelockData(bytes4 sig) public view returns (TimelockData memory) {
    return timelockData[sig];
}
```

## State Variable Reads

- **timelockData** (`mapping(bytes4 => struct Timelock.TimelockData)`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: Timelock.getTimelockData(bytes4) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
```

## Documentation

### Function Documentation

@notice Gets the timelock data for a specific function
