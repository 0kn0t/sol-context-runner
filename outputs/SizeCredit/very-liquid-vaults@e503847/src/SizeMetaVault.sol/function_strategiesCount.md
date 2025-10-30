# Function: strategiesCount()

**Contract**: [src/SizeMetaVault.sol/contract_SizeMetaVault.md]

## Metadata

- **Contract**: SizeMetaVault
- **Signature**: `strategiesCount()`
- **Visibility**: public
- **Source Range**: 18914:98:59

## Implementation

```solidity
/// @notice Returns the number of strategies in the vault
function strategiesCount() public view returns (uint256) {
    return strategies.length;
}
```

## State Variable Reads

- **strategies** (`contract IBaseVault[]`) [src/IBaseVault.sol/interface_IBaseVault.md]

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: SizeMetaVault.strategiesCount() (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
```

## Documentation

### Function Documentation

@notice Returns the number of strategies in the vault
