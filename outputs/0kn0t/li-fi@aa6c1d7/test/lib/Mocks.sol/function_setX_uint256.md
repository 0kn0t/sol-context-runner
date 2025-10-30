# Function: setX(uint256)

**Contract**: [test/lib/Mocks.sol/contract_LogicContract.md]

## Metadata

- **Contract**: LogicContract
- **Signature**: `setX(uint256)`
- **Visibility**: public
- **Source Range**: 6274:56:41

## Implementation

```solidity
/// @notice Set state variable x.
///  @dev Example: logic.setX(42);
function setX(uint256 _x) public {
    x = _x;
}
```

## State Variable Writes

- **x** (`uint256`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: LogicContract.setX(uint256) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
```

## Documentation

### Function Documentation

@notice Set state variable x.
 @dev Example: logic.setX(42);
