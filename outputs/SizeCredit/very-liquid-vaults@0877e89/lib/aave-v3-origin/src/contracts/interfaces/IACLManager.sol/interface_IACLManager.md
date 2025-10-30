# Interface: IACLManager

## Metadata

- **Name**: IACLManager
- **Type**: Interface
- **Path**: lib/aave-v3-origin/src/contracts/interfaces/IACLManager.sol
- **Documentation**:  @title IACLManager
   @author Aave
   @notice Defines the basic interface for the ACL Manager

## Public/External Functions

### ADDRESSES_PROVIDER()

- **Signature**: `ADDRESSES_PROVIDER()`
- **Visibility**: external
- **Source Range**: 395:77:12

**Signature:**
```solidity
///  @notice Returns the contract address of the PoolAddressesProvider
///  @return The address of the PoolAddressesProvider
function ADDRESSES_PROVIDER() external view returns (IPoolAddressesProvider);;
```

### POOL_ADMIN_ROLE()

- **Signature**: `POOL_ADMIN_ROLE()`
- **Visibility**: external
- **Source Range**: 588:59:12

**Signature:**
```solidity
///  @notice Returns the identifier of the PoolAdmin role
///  @return The id of the PoolAdmin role
function POOL_ADMIN_ROLE() external view returns (bytes32);;
```

### EMERGENCY_ADMIN_ROLE()

- **Signature**: `EMERGENCY_ADMIN_ROLE()`
- **Visibility**: external
- **Source Range**: 773:64:12

**Signature:**
```solidity
///  @notice Returns the identifier of the EmergencyAdmin role
///  @return The id of the EmergencyAdmin role
function EMERGENCY_ADMIN_ROLE() external view returns (bytes32);;
```

### RISK_ADMIN_ROLE()

- **Signature**: `RISK_ADMIN_ROLE()`
- **Visibility**: external
- **Source Range**: 953:59:12

**Signature:**
```solidity
///  @notice Returns the identifier of the RiskAdmin role
///  @return The id of the RiskAdmin role
function RISK_ADMIN_ROLE() external view returns (bytes32);;
```

### FLASH_BORROWER_ROLE()

- **Signature**: `FLASH_BORROWER_ROLE()`
- **Visibility**: external
- **Source Range**: 1136:63:12

**Signature:**
```solidity
///  @notice Returns the identifier of the FlashBorrower role
///  @return The id of the FlashBorrower role
function FLASH_BORROWER_ROLE() external view returns (bytes32);;
```

### BRIDGE_ROLE()

- **Signature**: `BRIDGE_ROLE()`
- **Visibility**: external
- **Source Range**: 1309:55:12

**Signature:**
```solidity
///  @notice Returns the identifier of the Bridge role
///  @return The id of the Bridge role
function BRIDGE_ROLE() external view returns (bytes32);;
```

### ASSET_LISTING_ADMIN_ROLE()

- **Signature**: `ASSET_LISTING_ADMIN_ROLE()`
- **Visibility**: external
- **Source Range**: 1496:68:12

**Signature:**
```solidity
///  @notice Returns the identifier of the AssetListingAdmin role
///  @return The id of the AssetListingAdmin role
function ASSET_LISTING_ADMIN_ROLE() external view returns (bytes32);;
```

### setRoleAdmin(bytes32,bytes32)

- **Signature**: `setRoleAdmin(bytes32,bytes32)`
- **Visibility**: external
- **Source Range**: 1805:64:12

**Signature:**
```solidity
///  @notice Set the role as admin of a specific role.
///  @dev By default the admin role for all roles is `DEFAULT_ADMIN_ROLE`.
///  @param role The role to be managed by the admin role
///  @param adminRole The admin role
function setRoleAdmin(bytes32 role, bytes32 adminRole) external;;
```

### addPoolAdmin(address)

- **Signature**: `addPoolAdmin(address)`
- **Visibility**: external
- **Source Range**: 1975:46:12

**Signature:**
```solidity
///  @notice Adds a new admin as PoolAdmin
///  @param admin The address of the new admin
function addPoolAdmin(address admin) external;;
```

### removePoolAdmin(address)

- **Signature**: `removePoolAdmin(address)`
- **Visibility**: external
- **Source Range**: 2133:49:12

**Signature:**
```solidity
///  @notice Removes an admin as PoolAdmin
///  @param admin The address of the admin to remove
function removePoolAdmin(address admin) external;;
```

### isPoolAdmin(address)

- **Signature**: `isPoolAdmin(address)`
- **Visibility**: external
- **Source Range**: 2377:65:12

**Signature:**
```solidity
///  @notice Returns true if the address is PoolAdmin, false otherwise
///  @param admin The address to check
///  @return True if the given address is PoolAdmin, false otherwise
function isPoolAdmin(address admin) external view returns (bool);;
```

### addEmergencyAdmin(address)

- **Signature**: `addEmergencyAdmin(address)`
- **Visibility**: external
- **Source Range**: 2553:51:12

**Signature:**
```solidity
///  @notice Adds a new admin as EmergencyAdmin
///  @param admin The address of the new admin
function addEmergencyAdmin(address admin) external;;
```

### removeEmergencyAdmin(address)

- **Signature**: `removeEmergencyAdmin(address)`
- **Visibility**: external
- **Source Range**: 2721:54:12

**Signature:**
```solidity
///  @notice Removes an admin as EmergencyAdmin
///  @param admin The address of the admin to remove
function removeEmergencyAdmin(address admin) external;;
```

### isEmergencyAdmin(address)

- **Signature**: `isEmergencyAdmin(address)`
- **Visibility**: external
- **Source Range**: 2980:70:12

**Signature:**
```solidity
///  @notice Returns true if the address is EmergencyAdmin, false otherwise
///  @param admin The address to check
///  @return True if the given address is EmergencyAdmin, false otherwise
function isEmergencyAdmin(address admin) external view returns (bool);;
```

### addRiskAdmin(address)

- **Signature**: `addRiskAdmin(address)`
- **Visibility**: external
- **Source Range**: 3156:46:12

**Signature:**
```solidity
///  @notice Adds a new admin as RiskAdmin
///  @param admin The address of the new admin
function addRiskAdmin(address admin) external;;
```

### removeRiskAdmin(address)

- **Signature**: `removeRiskAdmin(address)`
- **Visibility**: external
- **Source Range**: 3314:49:12

**Signature:**
```solidity
///  @notice Removes an admin as RiskAdmin
///  @param admin The address of the admin to remove
function removeRiskAdmin(address admin) external;;
```

### isRiskAdmin(address)

- **Signature**: `isRiskAdmin(address)`
- **Visibility**: external
- **Source Range**: 3558:65:12

**Signature:**
```solidity
///  @notice Returns true if the address is RiskAdmin, false otherwise
///  @param admin The address to check
///  @return True if the given address is RiskAdmin, false otherwise
function isRiskAdmin(address admin) external view returns (bool);;
```

### addFlashBorrower(address)

- **Signature**: `addFlashBorrower(address)`
- **Visibility**: external
- **Source Range**: 3746:53:12

**Signature:**
```solidity
///  @notice Adds a new address as FlashBorrower
///  @param borrower The address of the new FlashBorrower
function addFlashBorrower(address borrower) external;;
```

### removeFlashBorrower(address)

- **Signature**: `removeFlashBorrower(address)`
- **Visibility**: external
- **Source Range**: 3928:56:12

**Signature:**
```solidity
///  @notice Removes an address as FlashBorrower
///  @param borrower The address of the FlashBorrower to remove
function removeFlashBorrower(address borrower) external;;
```

### isFlashBorrower(address)

- **Signature**: `isFlashBorrower(address)`
- **Visibility**: external
- **Source Range**: 4190:72:12

**Signature:**
```solidity
///  @notice Returns true if the address is FlashBorrower, false otherwise
///  @param borrower The address to check
///  @return True if the given address is FlashBorrower, false otherwise
function isFlashBorrower(address borrower) external view returns (bool);;
```

### addBridge(address)

- **Signature**: `addBridge(address)`
- **Visibility**: external
- **Source Range**: 4369:44:12

**Signature:**
```solidity
///  @notice Adds a new address as Bridge
///  @param bridge The address of the new Bridge
function addBridge(address bridge) external;;
```

### removeBridge(address)

- **Signature**: `removeBridge(address)`
- **Visibility**: external
- **Source Range**: 4526:47:12

**Signature:**
```solidity
///  @notice Removes an address as Bridge
///  @param bridge The address of the bridge to remove
function removeBridge(address bridge) external;;
```

### isBridge(address)

- **Signature**: `isBridge(address)`
- **Visibility**: external
- **Source Range**: 4763:63:12

**Signature:**
```solidity
///  @notice Returns true if the address is Bridge, false otherwise
///  @param bridge The address to check
///  @return True if the given address is Bridge, false otherwise
function isBridge(address bridge) external view returns (bool);;
```

### addAssetListingAdmin(address)

- **Signature**: `addAssetListingAdmin(address)`
- **Visibility**: external
- **Source Range**: 4940:54:12

**Signature:**
```solidity
///  @notice Adds a new admin as AssetListingAdmin
///  @param admin The address of the new admin
function addAssetListingAdmin(address admin) external;;
```

### removeAssetListingAdmin(address)

- **Signature**: `removeAssetListingAdmin(address)`
- **Visibility**: external
- **Source Range**: 5114:57:12

**Signature:**
```solidity
///  @notice Removes an admin as AssetListingAdmin
///  @param admin The address of the admin to remove
function removeAssetListingAdmin(address admin) external;;
```

### isAssetListingAdmin(address)

- **Signature**: `isAssetListingAdmin(address)`
- **Visibility**: external
- **Source Range**: 5382:73:12

**Signature:**
```solidity
///  @notice Returns true if the address is AssetListingAdmin, false otherwise
///  @param admin The address to check
///  @return True if the given address is AssetListingAdmin, false otherwise
function isAssetListingAdmin(address admin) external view returns (bool);;
```
