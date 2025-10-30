# Function: rescueTokens(address,address)

**Contract**: [src/strategies/CashStrategyVault.sol/contract_CashStrategyVault.md]

## Metadata

- **Contract**: CashStrategyVault
- **Signature**: `rescueTokens(address,address)`
- **Visibility**: external
- **Source Range**: 8755:325:149
- **Inherited From**: BaseVault

## Implementation

```solidity
/// @notice Rescues tokens from the vault
///  @param token The address of the token to rescue
///  @param to The address to send the rescued tokens to
///  @dev Only addresses with GUARDIAN_ROLE can rescue tokens
///  @dev Reverts if the token is the address(0) or the asset of the vault
function rescueTokens(address token, address to) external onlyAuth(GUARDIAN_ROLE) {
    if (token == address(0)) revert NullAddress();
    if (token == address(asset())) revert InvalidAsset(token);
    uint256 amount = IERC20(token).balanceOf(address(this));
    IERC20(token).safeTransfer(to, amount);
}
```

## Related Implementations

### asset()

- **Kind**: internal
- **Source**: 6882:153:60
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/token/ERC20/extensions/ERC4626Upgradeable.sol:ERC4626Upgradeable:asset()`

```solidity
/// @dev See {IERC4626-asset}. 
function asset() virtual public view returns (address) {
    ERC4626Storage storage $ = _getERC4626Storage();
    return address($._asset);
}
```

### _getERC4626Storage()

- **Kind**: internal
- **Source**: 4088:159:60
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/token/ERC20/extensions/ERC4626Upgradeable.sol:ERC4626Upgradeable:_getERC4626Storage()`

```solidity
function _getERC4626Storage() private pure returns (ERC4626Storage storage $) {
    assembly {
        $.slot := ERC4626StorageLocation
    }
}
```

### onlyAuth(bytes32)

- **Kind**: modifier
- **Source**: 4783:80:149
- **Link**: `src/utils/BaseVault.sol:BaseVault:onlyAuth(bytes32)`

```solidity
/// @notice Modifier to restrict function access to addresses with specific roles
///  @dev Reverts if the caller doesn't have the required role
modifier onlyAuth(bytes32 role) {
    _checkAuthRole(role);
    _;
}
```

### _checkAuthRole(bytes32)

- **Kind**: internal
- **Source**: 5663:208:149
- **Link**: `src/utils/BaseVault.sol:BaseVault:_checkAuthRole(bytes32)`

```solidity
/// @notice Checks that the caller has the required role
///  @dev Reverts if the caller doesn't have the required role
function _checkAuthRole(bytes32 role) internal view {
    if (!auth().hasRole(role, _msgSender())) {
        revert IAccessControl.AccessControlUnauthorizedAccount(_msgSender(), role);
    }
}
```

### auth()

- **Kind**: internal
- **Source**: 14480:104:149
- **Link**: `src/utils/BaseVault.sol:BaseVault:auth()`

```solidity
/// @inheritdoc IVault
function auth() override public view returns (Auth) {
    return _getBaseVaultStorage()._auth;
}
```

### _getBaseVaultStorage()

- **Kind**: internal
- **Source**: 2552:165:149
- **Link**: `src/utils/BaseVault.sol:BaseVault:_getBaseVaultStorage()`

```solidity
function _getBaseVaultStorage() private pure returns (BaseVaultStorage storage $) {
    assembly {
        $.slot := BaseVaultStorageLocation
    }
}
```

### _msgSender()

- **Kind**: internal
- **Source**: 887:96:61
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/utils/ContextUpgradeable.sol:ContextUpgradeable:_msgSender()`

```solidity
function _msgSender() virtual internal view returns (address) {
    return msg.sender;
}
```

## External Calls

- **IERC20::balanceOf(address)**
- **IERC20::safeTransfer(contract IERC20,address,uint256)**

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: BaseVault.rescueTokens(address,address) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: external
  ├─ [1] ⚙️ FUNCTION: ERC4626Upgradeable.asset() (NodeID: 1)
  │   💬 Args: [no args]
  │   👁️  Def: public
  │ └─ [2] ⚙️ FUNCTION: ERC4626Upgradeable._getERC4626Storage() (NodeID: 2)
  │     💬 Args: [no args]
  │     👁️  Def: private
  └─ [1] 🔒 MODIFIER: BaseVault.onlyAuth(bytes32) (NodeID: 3)
      💬 Args: [GUARDIAN_ROLE]
    └─ [2] ⚙️ FUNCTION: BaseVault._checkAuthRole(bytes32) (NodeID: 4)
        💬 Args: [GUARDIAN_ROLE]
        👁️  Def: internal
      ├─ [3] ⚙️ FUNCTION: BaseVault.auth() (NodeID: 5)
      │   💬 Args: [no args]
      │   👁️  Def: public
      │ └─ [4] ⚙️ FUNCTION: BaseVault._getBaseVaultStorage() (NodeID: 6)
      │     💬 Args: [no args]
      │     👁️  Def: private
      ├─ [3] ⚙️ FUNCTION: ContextUpgradeable._msgSender() (NodeID: 7)
      │   💬 Args: [no args]
      │   👁️  Def: internal
      └─ [3] ⚙️ FUNCTION: ContextUpgradeable._msgSender() (NodeID: 8)
          💬 Args: [no args]
          👁️  Def: internal
```

## Documentation

### Function Documentation

@notice Rescues tokens from the vault
 @param token The address of the token to rescue
 @param to The address to send the rescued tokens to
 @dev Only addresses with GUARDIAN_ROLE can rescue tokens
 @dev Reverts if the token is the address(0) or the asset of the vault
