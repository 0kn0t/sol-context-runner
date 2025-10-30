# Function: setTotalAssetsCap(uint256)

**Contract**: [src/strategies/ERC4626StrategyVault.sol/contract_ERC4626StrategyVault.md]

## Metadata

- **Contract**: ERC4626StrategyVault
- **Signature**: `setTotalAssetsCap(uint256)`
- **Visibility**: external
- **Source Range**: 6051:139:56
- **Inherited From**: BaseVault

## Implementation

```solidity
/// @notice Sets the maximum total assets of the vault
///  @dev Only callable by the auth contract
///  @dev Lowering the total assets cap does not affect existing deposited assets
function setTotalAssetsCap(uint256 totalAssetsCap_) external onlyAuth(STRATEGIST_ROLE) {
    _setTotalAssetsCap(totalAssetsCap_);
}
```

## Related Implementations

### _setTotalAssetsCap(uint256)

- **Kind**: internal
- **Source**: 6255:230:56
- **Link**: `src/BaseVault.sol:BaseVault:_setTotalAssetsCap(uint256)`

```solidity
/// @notice Sets the maximum total assets of the vault
function _setTotalAssetsCap(uint256 totalAssetsCap_) private {
    uint256 oldTotalAssetsCap = totalAssetsCap;
    totalAssetsCap = totalAssetsCap_;
    emit TotalAssetsCapSet(oldTotalAssetsCap, totalAssetsCap_);
}
```

### onlyAuth(bytes32)

- **Kind**: modifier
- **Source**: 4644:193:56
- **Link**: `src/BaseVault.sol:BaseVault:onlyAuth(bytes32)`

```solidity
/// @notice Modifier to restrict function access to addresses with specific roles
///  @dev Reverts if the caller doesn't have the required role
modifier onlyAuth(bytes32 role) {
    if (!auth.hasRole(role, msg.sender)) {
        revert IAccessControl.AccessControlUnauthorizedAccount(msg.sender, role);
    }
    _;
}
```

## State Variable Reads

- **totalAssetsCap** (`uint256`)
- **auth** (`contract Auth`) [src/utils/Auth.sol/contract_Auth.md]

## State Variable Writes

- **totalAssetsCap** (`uint256`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: BaseVault.setTotalAssetsCap(uint256) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: external
  ├─ [1] ⚙️ FUNCTION: BaseVault._setTotalAssetsCap(uint256) (NodeID: 1)
  │   💬 Args: [totalAssetsCap_]
  │   👁️  Def: private
  └─ [1] 🔒 MODIFIER: BaseVault.onlyAuth(bytes32) (NodeID: 2)
      💬 Args: [STRATEGIST_ROLE]
```

## Documentation

### Function Documentation

@notice Sets the maximum total assets of the vault
 @dev Only callable by the auth contract
 @dev Lowering the total assets cap does not affect existing deposited assets
