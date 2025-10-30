# Function: updateEscrow(address)

**Contract**: [src/EbtcBSM.sol/contract_EbtcBSM.md]

## Metadata

- **Contract**: EbtcBSM
- **Signature**: `updateEscrow(address)`
- **Visibility**: external
- **Source Range**: 19016:745:40

## Implementation

```solidity
/// @notice Updates the escrow address and initiates an escrow migration
///  @dev Can only be called by authorized users
///  @param _newEscrow New escrow address
function updateEscrow(address _newEscrow) external requiresAuth() {
    require(_newEscrow != address(0), InvalidAddress());
    uint256 totalBalance = escrow.totalBalance();
    if (totalBalance > 0) {
        /// @dev cache deposit amount (will be set to 0 after migrateTo())
        uint256 totalAssetsDeposited = escrow.totalAssetsDeposited();
        /// @dev transfer liquidity to new vault
        escrow.onMigrateSource(_newEscrow);
        /// @dev set totalAssetsDeposited on the new vault (fee amount should be 0 here)
        IEscrow(_newEscrow).onMigrateTarget(totalAssetsDeposited);
    }
    emit EscrowUpdated(address(escrow), _newEscrow);
    escrow = IEscrow(_newEscrow);
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

## External Calls

- **IEscrow::totalBalance()**
- **IEscrow::totalAssetsDeposited()**
- **IEscrow::onMigrateSource(address)**
- **IEscrow::onMigrateTarget(uint256)**

## State Variable Reads

- **escrow** (`contract IEscrow`) [src/Dependencies/IEscrow.sol/interface_IEscrow.md]
- **_authority** (`contract Authority`) [src/Dependencies/Authority.sol/interface_Authority.md]

## State Variable Writes

- **escrow** (`contract IEscrow`) [src/Dependencies/IEscrow.sol/interface_IEscrow.md]

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: EbtcBSM.updateEscrow(address) (NodeID: 0)
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

@notice Updates the escrow address and initiates an escrow migration
 @dev Can only be called by authorized users
 @param _newEscrow New escrow address
