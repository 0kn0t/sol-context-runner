# Function: setMintingConfig(address,struct RateLimitingConstraint.MintingConfig)

**Contract**: [src/RateLimitingConstraint.sol/contract_RateLimitingConstraint.md]

## Metadata

- **Contract**: RateLimitingConstraint
- **Signature**: `setMintingConfig(address,struct RateLimitingConstraint.MintingConfig)`
- **Visibility**: external
- **Source Range**: 3795:335:42

## Implementation

```solidity
/// @notice Sets the minting configuration for a specific minter
///  @dev Can only be called by authorized users
///  @param _minter The address of the minter
///  @param _newMintingConfig The new minting configuration for the minter
function setMintingConfig(address _minter, MintingConfig calldata _newMintingConfig) external requiresAuth() {
    require(_newMintingConfig.relativeCapBPS <= BPS, InvalidMintingConfig());
    emit MintingConfigUpdated(_minter, mintingConfig[_minter], _newMintingConfig);
    mintingConfig[_minter] = _newMintingConfig;
}
```

## Related Implementations

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

- **BPS** (`uint256`)
- **mintingConfig** (`mapping(address => struct RateLimitingConstraint.MintingConfig)`)
- **_authority** (`contract Authority`) [src/Dependencies/Authority.sol/interface_Authority.md]

## State Variable Writes

- **mintingConfig** (`mapping(address => struct RateLimitingConstraint.MintingConfig)`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: RateLimitingConstraint.setMintingConfig(address,struct RateLimitingConstraint.MintingConfig) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: external
  └─ [1] 🔒 MODIFIER: AuthNoOwner.requiresAuth() (NodeID: 1)
      💬 Args: [no args]
    └─ [2] ⚙️ FUNCTION: AuthNoOwner.isAuthorized(address,bytes4) (NodeID: 2)
        💬 Args: [msg.sender, msg.sig]
        👁️  Def: internal
```

## Documentation

### Function Documentation

@notice Sets the minting configuration for a specific minter
 @dev Can only be called by authorized users
 @param _minter The address of the minter
 @param _newMintingConfig The new minting configuration for the minter
