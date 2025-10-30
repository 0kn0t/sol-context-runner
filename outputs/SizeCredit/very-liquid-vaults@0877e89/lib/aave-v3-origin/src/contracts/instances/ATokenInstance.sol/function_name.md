# Function: name()

**Contract**: [lib/aave-v3-origin/src/contracts/instances/ATokenInstance.sol/contract_ATokenInstance.md]

## Metadata

- **Contract**: ATokenInstance
- **Signature**: `name()`
- **Visibility**: public
- **Source Range**: 2864:84:35
- **Inherited From**: IncentivizedERC20

## Implementation

```solidity
/// @inheritdoc IERC20Detailed
function name() override public view returns (string memory) {
    return _name;
}
```

## State Variable Reads

- **_name** (`string`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: IncentivizedERC20.name() (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
```

## Documentation

### Function Documentation

@inheritdoc IERC20Detailed
