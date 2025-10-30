# Function: constructor(address,address,address,address,address)

**Contract**: [src/ERC4626Escrow.sol/contract_ERC4626Escrow.md]

## Metadata

- **Contract**: ERC4626Escrow
- **Signature**: `constructor(address,address,address,address,address)`
- **Visibility**: public
- **Source Range**: 1580:277:39

## Implementation

```solidity
/// @notice Constructor for the contract
///  @param _externalVault The address of the ERC4626 compliant external vault
///  @param _assetToken The ERC20 token address used for deposits and withdrawals
///  @param _bsm The address of the BSM
///  @param _governance The governance address used for AuthNoOwner
///  @param _feeRecipient The address where collected fees are sent
constructor(address _externalVault, address _assetToken, address _bsm, address _governance, address _feeRecipient) BaseEscrow(_assetToken,_bsm,_governance,_feeRecipient) {
    EXTERNAL_VAULT = IERC4626(_externalVault);
}
```

## Related Implementations

### (address,address,address,address)

- **Kind**: internal
- **Source**: 1239:369:22
- **Link**: `src/BaseEscrow.sol:BaseEscrow:constructor(address,address,address,address)`

```solidity
/// @notice Contract constructor
///  @param _assetToken The ERC20 token address used for deposits and withdrawals
///  @param _bsm The address of the eBTC Stability Module (BSM)
///  @param _governance The governance address used for AuthNoOwner
///  @param _feeRecipient The address where collected fees are sent
constructor(address _assetToken, address _bsm, address _governance, address _feeRecipient) {
    ASSET_TOKEN = IERC20(_assetToken);
    BSM = _bsm;
    FEE_RECIPIENT = _feeRecipient;
    _initializeAuthority(_governance);
    ASSET_TOKEN.forceApprove(BSM, type(uint256).max);
}
```

### _initializeAuthority(address)

- **Kind**: internal
- **Source**: 1689:388:25
- **Link**: `src/Dependencies/AuthNoOwner.sol:AuthNoOwner:_initializeAuthority(address)`

```solidity
/// @notice Changed constructor to initialize to allow flexibility of constructor vs initializer use
///  @notice sets authorityInitialized flag to ensure only one use of
function _initializeAuthority(address newAuthority) internal {
    require(address(_authority) == address(0), "Auth: authority is non-zero");
    require(!_authorityInitialized, "Auth: authority already initialized");
    _authority = Authority(newAuthority);
    _authorityInitialized = true;
    emit AuthorityUpdated(address(this), Authority(newAuthority));
}
```

## State Variable Reads

- **ASSET_TOKEN** (`contract IERC20`) [lib/openzeppelin-contracts/contracts/token/ERC20/IERC20.sol/interface_IERC20.md]
- **BSM** (`address`)
- **_authority** (`contract Authority`) [src/Dependencies/Authority.sol/interface_Authority.md]
- **_authorityInitialized** (`bool`)

## State Variable Writes

- **EXTERNAL_VAULT** (`contract IERC4626`) [lib/openzeppelin-contracts/contracts/interfaces/IERC4626.sol/interface_IERC4626.md]
- **ASSET_TOKEN** (`contract IERC20`) [lib/openzeppelin-contracts/contracts/token/ERC20/IERC20.sol/interface_IERC20.md]
- **BSM** (`address`)
- **FEE_RECIPIENT** (`address`)
- **_authority** (`contract Authority`) [src/Dependencies/Authority.sol/interface_Authority.md]
- **_authorityInitialized** (`bool`)

## Call Tree

```
┌─ [0] 🏗️ CONSTRUCTOR: ERC4626Escrow.constructor(address,address,address,address,address) (NodeID: 0)
    💬 Args: [no args]
    🏗️  Contract: ERC4626Escrow
  └─ [1] 🏗️ CONSTRUCTOR: BaseEscrow.constructor(address,address,address,address) (NodeID: 1)
      💬 Args: [_assetToken, _bsm, _governance, _feeRecipient]
      🏗️  Contract: BaseEscrow
    └─ [2] ⚙️ FUNCTION: AuthNoOwner._initializeAuthority(address) (NodeID: 2)
        💬 Args: [_governance]
        👁️  Def: internal
```

## Documentation

### Function Documentation

@notice Constructor for the contract
 @param _externalVault The address of the ERC4626 compliant external vault
 @param _assetToken The ERC20 token address used for deposits and withdrawals
 @param _bsm The address of the BSM
 @param _governance The governance address used for AuthNoOwner
 @param _feeRecipient The address where collected fees are sent
