# Function: updateMaxLendPerTokenBulk(address[],uint256[])

**Contract**: [src/TokenHolder.sol/contract_TokenHolder.md]

## Metadata

- **Contract**: TokenHolder
- **Signature**: `updateMaxLendPerTokenBulk(address[],uint256[])`
- **Visibility**: public
- **Source Range**: 12184:1051:13

## Implementation

```solidity
function updateMaxLendPerTokenBulk(address[] memory collateralAddresses, uint256[] memory newMaxLendPerTokens) public onlyRole(DEFAULT_ADMIN_ROLE) {
    require(collateralAddresses.length == newMaxLendPerTokens.length, "Array lengths must match");
    for (uint256 i = 0; i < collateralAddresses.length; i++) {
        address collateralAddress = collateralAddresses[i];
        uint256 newMaxLendPerToken = newMaxLendPerTokens[i];
        require(collateralMapping[collateralAddress].active, "Collateral not found or not active");
        uint256 oldMaxLendPerToken = collateralMapping[collateralAddress].maxLendPerToken;
        collateralMapping[collateralAddress].maxLendPerToken = newMaxLendPerToken;
        emit MaxLendPerTokenUpdated(collateralAddress, oldMaxLendPerToken, newMaxLendPerToken);
    }
}
```

## Related Implementations

### onlyRole(bytes32)

- **Kind**: modifier
- **Source**: 3251:76:0
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/access/AccessControlUpgradeable.sol:AccessControlUpgradeable:onlyRole(bytes32)`

```solidity
///  @dev Modifier that checks that an account has a specific role. Reverts
///  with an {AccessControlUnauthorizedAccount} error including the required role.
modifier onlyRole(bytes32 role) {
    _checkRole(role);
    _;
}
```

### _checkRole(bytes32)

- **Kind**: internal
- **Source**: 4217:103:0
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/access/AccessControlUpgradeable.sol:AccessControlUpgradeable:_checkRole(bytes32)`

```solidity
///  @dev Reverts with an {AccessControlUnauthorizedAccount} error if `_msgSender()`
///  is missing `role`. Overriding this function changes the behavior of the {onlyRole} modifier.
function _checkRole(bytes32 role) virtual internal view {
    _checkRole(role, _msgSender());
}
```

### _checkRole(bytes32,address)

- **Kind**: internal
- **Source**: 4450:197:0
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/access/AccessControlUpgradeable.sol:AccessControlUpgradeable:_checkRole(bytes32,address)`

```solidity
///  @dev Reverts with an {AccessControlUnauthorizedAccount} error if `account`
///  is missing `role`.
function _checkRole(bytes32 role, address account) virtual internal view {
    if (!hasRole(role, account)) {
        revert AccessControlUnauthorizedAccount(account, role);
    }
}
```

### _msgSender()

- **Kind**: internal
- **Source**: 908:96:2
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/utils/ContextUpgradeable.sol:ContextUpgradeable:_msgSender()`

```solidity
function _msgSender() virtual internal view returns (address) {
    return msg.sender;
}
```

### hasRole(bytes32,address)

- **Kind**: internal
- **Source**: 3801:207:0
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/access/AccessControlUpgradeable.sol:AccessControlUpgradeable:hasRole(bytes32,address)`

```solidity
///  @dev Returns `true` if `account` has been granted `role`.
function hasRole(bytes32 role, address account) virtual public view returns (bool) {
    AccessControlStorage storage $ = _getAccessControlStorage();
    return $._roles[role].hasRole[account];
}
```

### _getAccessControlStorage()

- **Kind**: internal
- **Source**: 2889:177:0
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/access/AccessControlUpgradeable.sol:AccessControlUpgradeable:_getAccessControlStorage()`

```solidity
function _getAccessControlStorage() private pure returns (AccessControlStorage storage $) {
    assembly {
        $.slot := AccessControlStorageLocation
    }
}
```

## State Variable Reads

- **collateralMapping** (`mapping(address => struct Collateral)`)

## State Variable Writes

- **collateralMapping** (`mapping(address => struct Collateral)`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: TokenHolder.updateMaxLendPerTokenBulk(address[],uint256[]) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
  └─ [1] 🔒 MODIFIER: AccessControlUpgradeable.onlyRole(bytes32) (NodeID: 1)
      💬 Args: [DEFAULT_ADMIN_ROLE]
    └─ [2] ⚙️ FUNCTION: AccessControlUpgradeable._checkRole(bytes32) (NodeID: 2)
        💬 Args: [DEFAULT_ADMIN_ROLE]
        👁️  Def: internal
      └─ [3] ⚙️ FUNCTION: AccessControlUpgradeable._checkRole(bytes32,address) (NodeID: 3)
          💬 Args: [role, _msgSender()]
          👁️  Def: internal
        ├─ [4] ⚙️ FUNCTION: ContextUpgradeable._msgSender() (NodeID: 6)
        │   💬 Args: [no args]
        │   👁️  Def: internal
        └─ [4] ⚙️ FUNCTION: AccessControlUpgradeable.hasRole(bytes32,address) (NodeID: 4)
            💬 Args: [role, account]
            👁️  Def: public
          └─ [5] ⚙️ FUNCTION: AccessControlUpgradeable._getAccessControlStorage() (NodeID: 5)
              💬 Args: [no args]
              👁️  Def: private
```
