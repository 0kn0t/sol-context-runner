# Function: isStrategy(contract IBaseVault)

**Contract**: [src/SizeMetaVault.sol/contract_SizeMetaVault.md]

## Metadata

- **Contract**: SizeMetaVault
- **Signature**: `isStrategy(contract IBaseVault)`
- **Visibility**: public
- **Source Range**: 19079:286:59

## Implementation

```solidity
/// @notice Returns true if the strategy is in the vault
function isStrategy(IBaseVault strategy) public view returns (bool) {
    uint256 length = strategies.length;
    for (uint256 i = 0; i < length; i++) {
        if (strategies[i] == strategy) {
            return true;
        }
    }
    return false;
}
```

## State Variable Reads

- **strategies** (`contract IBaseVault[]`) [src/IBaseVault.sol/interface_IBaseVault.md]

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: SizeMetaVault.isStrategy(contract IBaseVault) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
```

## Documentation

### Function Documentation

@notice Returns true if the strategy is in the vault
