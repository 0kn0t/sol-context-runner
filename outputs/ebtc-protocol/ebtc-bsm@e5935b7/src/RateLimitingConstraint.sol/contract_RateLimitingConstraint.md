# Contract: RateLimitingConstraint

## Metadata

- **Name**: RateLimitingConstraint
- **Type**: Contract
- **Path**: src/RateLimitingConstraint.sol
- **Documentation**: @title Rate Limiting Constraint for Minting
   @notice This contract enforces rate-limiting constraints on minting operations to control inflation and supply of tokens.

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
/// @notice Basis points constant for percentage calculations
uint256 public constant BPS = 10_000
```

### mintingConfig

```solidity
/// @notice Mapping of minter addresses to their minting configurations
mapping(address => MintingConfig) internal mintingConfig
```

### ACTIVE_POOL_OBSERVER

```solidity
/// @notice The observer interface to interact with the active pool
IActivePoolObserver public immutable ACTIVE_POOL_OBSERVER
```

**IActivePoolObserver**: [src/Dependencies/IActivePoolObserver.sol/interface_IActivePoolObserver.md]

## Structs

### MintingConfig

```solidity
/// @notice Minting configuration structure for each minter
struct MintingConfig {
    uint256 relativeCapBPS;
    uint256 absoluteCap;
    bool useAbsoluteCap;
}
```

## Errors

### ConstraintCheckFailed (inherited from IConstraint)

```solidity
error ConstraintCheckFailed(address constraint, uint256 amount, address bsm, bytes errData);
```

### AboveMintingCap

```solidity
/// @notice Error thrown when a minting request exceeds the configured cap
error AboveMintingCap(uint256 amountToMint, uint256 newTotalToMint, uint256 maxMint);
```

### InvalidMintingConfig

```solidity
/// @notice Error thrown when trying to set an invalid minting config
error InvalidMintingConfig();
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

### MintingConfigUpdated

```solidity
/// @notice Event emitted when a minter's configuration is updated
event MintingConfigUpdated(address indexed minter, MintingConfig oldConfig, MintingConfig newConfig);
```

## Public/External Functions

### constructor(address,address)

- **Signature**: `constructor(address,address)`
- **Visibility**: public
- **Source Range**: 1929:185:42
- **Details**: [function_constructor_address_address.md](./function_constructor_address_address.md)

**Signature:**
```solidity
/// @notice Contract constructor
///  @param _activePoolObserver Address of the active pool observer
///  @param _governance Address of the governance mechanism
constructor(address _activePoolObserver, address _governance);
```

### canProcess(uint256,address)

- **Signature**: `canProcess(uint256,address)`
- **Visibility**: external
- **Source Range**: 2445:765:42
- **Details**: [function_canProcess_uint256_address.md](./function_canProcess_uint256_address.md)

**Signature:**
```solidity
/// @notice Checks if the minting amount is within the allowed cap for the minter
///  @param _amount The amount to be minted
///  @param _bsm The address of the minter
///  @return bool True if the minting is within the cap, false otherwise
///  @return bytes Encoded error data if the mint is above the cap
function canProcess(uint256 _amount, address _bsm) external view returns (bool, bytes memory);
```

### getMintingConfig(address)

- **Signature**: `getMintingConfig(address)`
- **Visibility**: external
- **Source Range**: 3407:134:42
- **Details**: [function_getMintingConfig_address.md](./function_getMintingConfig_address.md)

**Signature:**
```solidity
/// @notice Returns the minting configuration for a specific minter
///  @param _minter The address of the minter
///  @return MintingConfig The minting configuration of the minter
function getMintingConfig(address _minter) external view returns (MintingConfig memory);
```

### setMintingConfig(address,struct RateLimitingConstraint.MintingConfig)

- **Signature**: `setMintingConfig(address,struct RateLimitingConstraint.MintingConfig)`
- **Visibility**: external
- **Source Range**: 3795:335:42
- **Details**: [function_setMintingConfig_address_struct_RateLimitingConstraint.MintingConfig.md](./function_setMintingConfig_address_struct_RateLimitingConstraint.MintingConfig.md)

**Signature:**
```solidity
/// @notice Sets the minting configuration for a specific minter
///  @dev Can only be called by authorized users
///  @param _minter The address of the minter
///  @param _newMintingConfig The new minting configuration for the minter
function setMintingConfig(address _minter, MintingConfig calldata _newMintingConfig) external requiresAuth();
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
