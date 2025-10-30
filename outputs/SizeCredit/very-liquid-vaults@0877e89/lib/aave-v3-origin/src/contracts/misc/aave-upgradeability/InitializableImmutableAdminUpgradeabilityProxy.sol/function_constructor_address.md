# Function: constructor(address)

**Contract**: [lib/aave-v3-origin/src/contracts/misc/aave-upgradeability/InitializableImmutableAdminUpgradeabilityProxy.sol/contract_InitializableImmutableAdminUpgradeabilityProxy.md]

## Metadata

- **Contract**: InitializableImmutableAdminUpgradeabilityProxy
- **Signature**: `constructor(address)`
- **Visibility**: public
- **Source Range**: 735:109:23

## Implementation

```solidity
///  @dev Constructor.
///  @param admin The address of the admin
constructor(address admin) BaseImmutableAdminUpgradeabilityProxy(admin) {}
```

## Related Implementations

### (address)

- **Kind**: internal
- **Source**: 908:54:22
- **Link**: `lib/aave-v3-origin/src/contracts/misc/aave-upgradeability/BaseImmutableAdminUpgradeabilityProxy.sol:BaseImmutableAdminUpgradeabilityProxy:constructor(address)`

```solidity
///  @dev Constructor.
///  @param admin_ The address of the admin
constructor(address admin_) {
    _admin = admin_;
}
```

## State Variable Writes

- **_admin** (`address`)

## Call Tree

```
┌─ [0] 🏗️ CONSTRUCTOR: InitializableImmutableAdminUpgradeabilityProxy.constructor(address) (NodeID: 0)
    💬 Args: [no args]
    🏗️  Contract: InitializableImmutableAdminUpgradeabilityProxy
  └─ [1] 🏗️ CONSTRUCTOR: BaseImmutableAdminUpgradeabilityProxy.constructor(address) (NodeID: 1)
      💬 Args: [admin]
      🏗️  Contract: BaseImmutableAdminUpgradeabilityProxy
```

## Documentation

### Function Documentation

 @dev Constructor.
 @param admin The address of the admin
