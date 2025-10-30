# Function: constructor(address,address,address,address,address,address)

**Contract**: [src/EbtcBSM.sol/contract_EbtcBSM.md]

## Metadata

- **Contract**: EbtcBSM
- **Signature**: `constructor(address,address,address,address,address,address)`
- **Visibility**: public
- **Source Range**: 3171:975:40

## Implementation

```solidity
/// @notice Constructs the EbtcBSM contract
///  @param _assetToken Address of the underlying asset token
///  @param _oraclePriceConstraint Address of the oracle price constraint
///  @param _rateLimitingConstraint Address of the rate limiting constraint
///  @param _ebtcToken Address of the eBTC token
///  @param _governance Address of the governor
constructor(address _assetToken, address _oraclePriceConstraint, address _rateLimitingConstraint, address _buyAssetConstraint, address _ebtcToken, address _governance) {
    require(_assetToken != address(0));
    require(_oraclePriceConstraint != address(0));
    require(_rateLimitingConstraint != address(0));
    require(_buyAssetConstraint != address(0));
    require(_ebtcToken != address(0));
    require(_governance != address(0));
    ASSET_TOKEN = IERC20(_assetToken);
    ASSET_TOKEN_PRECISION = 10 ** ERC20(_assetToken).decimals();
    require(ASSET_TOKEN_PRECISION <= 1e18);
    oraclePriceConstraint = IConstraint(_oraclePriceConstraint);
    rateLimitingConstraint = IConstraint(_rateLimitingConstraint);
    buyAssetConstraint = IConstraint(_buyAssetConstraint);
    EBTC_TOKEN = IEbtcToken(_ebtcToken);
    _initializeAuthority(_governance);
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

- **ERC20::decimals()**

## State Variable Reads

- **ASSET_TOKEN_PRECISION** (`uint256`)
- **_authority** (`contract Authority`) [src/Dependencies/Authority.sol/interface_Authority.md]
- **_authorityInitialized** (`bool`)

## State Variable Writes

- **ASSET_TOKEN** (`contract IERC20`) [lib/openzeppelin-contracts/contracts/token/ERC20/IERC20.sol/interface_IERC20.md]
- **ASSET_TOKEN_PRECISION** (`uint256`)
- **oraclePriceConstraint** (`contract IConstraint`) [src/Dependencies/IConstraint.sol/interface_IConstraint.md]
- **rateLimitingConstraint** (`contract IConstraint`) [src/Dependencies/IConstraint.sol/interface_IConstraint.md]
- **buyAssetConstraint** (`contract IConstraint`) [src/Dependencies/IConstraint.sol/interface_IConstraint.md]
- **EBTC_TOKEN** (`contract IEbtcToken`) [src/Dependencies/IEbtcToken.sol/interface_IEbtcToken.md]
- **_authority** (`contract Authority`) [src/Dependencies/Authority.sol/interface_Authority.md]
- **_authorityInitialized** (`bool`)

## Call Tree

```
┌─ [0] 🏗️ CONSTRUCTOR: EbtcBSM.constructor(address,address,address,address,address,address) (NodeID: 0)
    💬 Args: [no args]
    🏗️  Contract: EbtcBSM
  └─ [1] ⚙️ FUNCTION: AuthNoOwner._initializeAuthority(address) (NodeID: 1)
      💬 Args: [_governance]
      👁️  Def: internal
```

## Documentation

### Function Documentation

@notice Constructs the EbtcBSM contract
 @param _assetToken Address of the underlying asset token
 @param _oraclePriceConstraint Address of the oracle price constraint
 @param _rateLimitingConstraint Address of the rate limiting constraint
 @param _ebtcToken Address of the eBTC token
 @param _governance Address of the governor
