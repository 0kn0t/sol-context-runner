# Function: getMintingConfig(address)

**Contract**: [src/RateLimitingConstraint.sol/contract_RateLimitingConstraint.md]

## Metadata

- **Contract**: RateLimitingConstraint
- **Signature**: `getMintingConfig(address)`
- **Visibility**: external
- **Source Range**: 3407:134:42

## Implementation

```solidity
/// @notice Returns the minting configuration for a specific minter
///  @param _minter The address of the minter
///  @return MintingConfig The minting configuration of the minter
function getMintingConfig(address _minter) external view returns (MintingConfig memory) {
    return mintingConfig[_minter];
}
```

## State Variable Reads

- **mintingConfig** (`mapping(address => struct RateLimitingConstraint.MintingConfig)`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: RateLimitingConstraint.getMintingConfig(address) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: external
```

## Documentation

### Function Documentation

@notice Returns the minting configuration for a specific minter
 @param _minter The address of the minter
 @return MintingConfig The minting configuration of the minter
