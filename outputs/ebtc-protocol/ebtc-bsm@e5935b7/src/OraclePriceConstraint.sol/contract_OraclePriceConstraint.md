# Contract: OraclePriceConstraint

## Metadata

- **Name**: OraclePriceConstraint
- **Type**: Contract
- **Path**: src/OraclePriceConstraint.sol
- **Documentation**: @title Oracle Price Constraint for Minting
   @notice This contract uses price feed from an oracle to set constraints on minting based on the asset's current market price.
   @dev Implements IConstraint to provide minting restrictions based on real-time asset price information provided by Chainlink oracles.

## Implements Interfaces

- **IConstraint** [src/Dependencies/IConstraint.sol/interface_IConstraint.md]

## State Variables

### _authority (inherited from AuthNoOwner)

```solidity
Authority private _authority
```

**Authority**: [src/Dependencies/Authority.sol/interface_Authority.md]

### _authorityInitialized (inherited from AuthNoOwner)

```solidity
bool private _authorityInitialized
```

### BPS

```solidity
/// @notice Basis points constant for price calculations
uint256 public constant BPS = 10_000
```

### ASSET_FEED

```solidity
/// @notice Asset feed is denominated in BTC (i.e. tBTC/BTC)
AggregatorV3Interface public immutable ASSET_FEED
```

**AggregatorV3Interface**: [src/Dependencies/AggregatorV3Interface.sol/interface_AggregatorV3Interface.md]

### ASSET_FEED_PRECISION

```solidity
/// @notice Precision of the asset price from the feed
uint256 public immutable ASSET_FEED_PRECISION
```

### minPriceBPS

```solidity
/// @notice Minimum price, in basis points, below which minting is not allowed
uint256 public minPriceBPS
```

### oracleFreshnessSeconds

```solidity
/// @notice Maximum allowable age of the latest oracle price
uint256 public oracleFreshnessSeconds
```

## Errors

### ConstraintCheckFailed (inherited from IConstraint)

```solidity
error ConstraintCheckFailed(address constraint, uint256 amount, address bsm, bytes errData);
```

### BadOraclePrice

```solidity
/// @notice Error thrown when the oracle price is invalid (non-positive)
error BadOraclePrice(int256 price);
```

### StaleOraclePrice

```solidity
/// @notice Error thrown when the latest oracle price is too old
error StaleOraclePrice(uint256 updatedAt);
```

### BelowMinPrice

```solidity
/// @notice Error thrown when the asset price is below the minimum required for minting
error BelowMinPrice(uint256 assetPrice, uint256 minPrice);
```

### InvalidMinPrice

```solidity
/// @notice Error thrown when trying to set an invalid min price
error InvalidMinPrice();
```

## Events

### ConstraintUpdated (inherited from IConstraint)

```solidity
event ConstraintUpdated(address indexed oldConstraint, address indexed newConstraint);
```

### AuthorityUpdated (inherited from AuthNoOwner)

```solidity
event AuthorityUpdated(address indexed user, Authority indexed newAuthority);
```

### MinPriceUpdated

```solidity
/// @notice Event emitted when the minimum price is updated
event MinPriceUpdated(uint256 oldMinPrice, uint256 newMinPrice);
```

### OracleFreshnessUpdated

```solidity
/// @notice Event emitted when the oracle freshness requirement is updated
event OracleFreshnessUpdated(uint256 oldOracleFreshness, uint256 newOracleFreshness);
```

## Public/External Functions

### constructor(address,address)

- **Signature**: `constructor(address,address)`
- **Visibility**: public
- **Source Range**: 2182:328:41
- **Details**: [function_constructor_address_address.md](./function_constructor_address_address.md)

**Signature:**
```solidity
/// @notice Contract constructor
///  @param _assetFeed Address of the oracle price feed
///  @param _governance Address of the governance authority
constructor(address _assetFeed, address _governance);
```

### canProcess(uint256,address)

- **Signature**: `canProcess(uint256,address)`
- **Visibility**: external
- **Source Range**: 3571:609:41
- **Details**: [function_canProcess_uint256_address.md](./function_canProcess_uint256_address.md)

**Signature:**
```solidity
/// @notice Determines if minting is allowed based on the current asset price
///  @param _amount The amount of tokens requested to mint (unused in this contract)
///  @param _bsm The address requesting to mint (unused in this contract)
///  @return bool True if minting is allowed, false otherwise
///  @return bytes Encoded error data if minting is not allowed
function canProcess(uint256 _amount, address _bsm) external view returns (bool, bytes memory);
```

### setMinPrice(uint256)

- **Signature**: `setMinPrice(uint256)`
- **Visibility**: external
- **Source Range**: 4369:222:41
- **Details**: [function_setMinPrice_uint256.md](./function_setMinPrice_uint256.md)

**Signature:**
```solidity
/// @notice Updates the minimum price threshold for minting
///  @dev Can only be called by authorized users
///  @param _minPriceBPS The new minimum price, in basis points
function setMinPrice(uint256 _minPriceBPS) external requiresAuth();
```

### setOracleFreshness(uint256)

- **Signature**: `setOracleFreshness(uint256)`
- **Visibility**: external
- **Source Range**: 4786:282:41
- **Details**: [function_setOracleFreshness_uint256.md](./function_setOracleFreshness_uint256.md)

**Signature:**
```solidity
/// @notice Updates the maximum age for acceptable oracle data
///  @dev Can only be called by authorized users
///  @param _oracleFreshnessSeconds The new maximum age in seconds
function setOracleFreshness(uint256 _oracleFreshnessSeconds) external requiresAuth();
```

### authority() (inherited from AuthNoOwner)

- **Signature**: `authority()`
- **Visibility**: public
- **Source Range**: 778:87:25
- **Details**: [function_authority.md](./function_authority.md)

**Signature:**
```solidity
function authority() public view returns (Authority);
```

### authorityInitialized() (inherited from AuthNoOwner)

- **Signature**: `authorityInitialized()`
- **Visibility**: public
- **Source Range**: 871:104:25
- **Details**: [function_authorityInitialized.md](./function_authorityInitialized.md)

**Signature:**
```solidity
function authorityInitialized() public view returns (bool);
```
