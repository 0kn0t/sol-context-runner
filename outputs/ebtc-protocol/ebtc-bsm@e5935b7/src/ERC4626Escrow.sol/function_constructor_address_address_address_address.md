# Function: constructor(address,address,address,address)

**Contract**: [src/ERC4626Escrow.sol/contract_ERC4626Escrow.md]

## Metadata

- **Contract**: ERC4626Escrow
- **Signature**: `constructor(address,address,address,address)`
- **Visibility**: public
- **Source Range**: 1239:369:22
- **Inherited From**: BaseEscrow

## Implementation

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

## Related Implementations

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

## External Calls

- **IERC20::forceApprove(contract IERC20,address,uint256)**

## State Variable Reads

- **ASSET_TOKEN** (`contract IERC20`) [lib/openzeppelin-contracts/contracts/token/ERC20/IERC20.sol/interface_IERC20.md]
- **BSM** (`address`)
- **_authority** (`contract Authority`) [src/Dependencies/Authority.sol/interface_Authority.md]
- **_authorityInitialized** (`bool`)

## State Variable Writes

- **ASSET_TOKEN** (`contract IERC20`) [lib/openzeppelin-contracts/contracts/token/ERC20/IERC20.sol/interface_IERC20.md]
- **BSM** (`address`)
- **FEE_RECIPIENT** (`address`)
- **_authority** (`contract Authority`) [src/Dependencies/Authority.sol/interface_Authority.md]
- **_authorityInitialized** (`bool`)

## Call Tree

```
┌─ [0] 🏗️ CONSTRUCTOR: BaseEscrow.constructor(address,address,address,address) (NodeID: 0)
    💬 Args: [no args]
    🏗️  Contract: BaseEscrow
  └─ [1] ⚙️ FUNCTION: AuthNoOwner._initializeAuthority(address) (NodeID: 1)
      💬 Args: [_governance]
      👁️  Def: internal
```

## Documentation

### Function Documentation

@notice Contract constructor
 @param _assetToken The ERC20 token address used for deposits and withdrawals
 @param _bsm The address of the eBTC Stability Module (BSM)
 @param _governance The governance address used for AuthNoOwner
 @param _feeRecipient The address where collected fees are sent
