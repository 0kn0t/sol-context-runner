# Function: constructor(address,address)

**Contract**: [src/OraclePriceConstraint.sol/contract_OraclePriceConstraint.md]

## Metadata

- **Contract**: OraclePriceConstraint
- **Signature**: `constructor(address,address)`
- **Visibility**: public
- **Source Range**: 2182:328:41

## Implementation

```solidity
/// @notice Contract constructor
///  @param _assetFeed Address of the oracle price feed
///  @param _governance Address of the governance authority
constructor(address _assetFeed, address _governance) {
    ASSET_FEED = AggregatorV3Interface(_assetFeed);
    ASSET_FEED_PRECISION = 10 ** ASSET_FEED.decimals();
    _initializeAuthority(_governance);
    minPriceBPS = BPS;
    oracleFreshnessSeconds = 1 days;
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

- **AggregatorV3Interface::decimals()**

## State Variable Reads

- **ASSET_FEED** (`contract AggregatorV3Interface`) [src/Dependencies/AggregatorV3Interface.sol/interface_AggregatorV3Interface.md]
- **BPS** (`uint256`)
- **_authority** (`contract Authority`) [src/Dependencies/Authority.sol/interface_Authority.md]
- **_authorityInitialized** (`bool`)

## State Variable Writes

- **ASSET_FEED** (`contract AggregatorV3Interface`) [src/Dependencies/AggregatorV3Interface.sol/interface_AggregatorV3Interface.md]
- **ASSET_FEED_PRECISION** (`uint256`)
- **minPriceBPS** (`uint256`)
- **oracleFreshnessSeconds** (`uint256`)
- **_authority** (`contract Authority`) [src/Dependencies/Authority.sol/interface_Authority.md]
- **_authorityInitialized** (`bool`)

## Call Tree

```
┌─ [0] 🏗️ CONSTRUCTOR: OraclePriceConstraint.constructor(address,address) (NodeID: 0)
    💬 Args: [no args]
    🏗️  Contract: OraclePriceConstraint
  └─ [1] ⚙️ FUNCTION: AuthNoOwner._initializeAuthority(address) (NodeID: 1)
      💬 Args: [_governance]
      👁️  Def: internal
```

## Documentation

### Function Documentation

@notice Contract constructor
 @param _assetFeed Address of the oracle price feed
 @param _governance Address of the governance authority
