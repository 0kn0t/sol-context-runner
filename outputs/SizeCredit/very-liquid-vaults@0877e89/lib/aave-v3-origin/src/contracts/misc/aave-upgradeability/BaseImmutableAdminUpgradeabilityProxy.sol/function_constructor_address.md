# Function: constructor(address)

**Contract**: [lib/aave-v3-origin/src/contracts/misc/aave-upgradeability/BaseImmutableAdminUpgradeabilityProxy.sol/contract_BaseImmutableAdminUpgradeabilityProxy.md]

## Metadata

- **Contract**: BaseImmutableAdminUpgradeabilityProxy
- **Signature**: `constructor(address)`
- **Visibility**: public
- **Source Range**: 908:54:22

## Implementation

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
┌─ [0] 🏗️ CONSTRUCTOR: BaseImmutableAdminUpgradeabilityProxy.constructor(address) (NodeID: 0)
    💬 Args: [no args]
    🏗️  Contract: BaseImmutableAdminUpgradeabilityProxy
```

## Documentation

### Function Documentation

 @dev Constructor.
 @param admin_ The address of the admin
