# Function: constructor(address,address)

**Contract**: [src/RateLimitingConstraint.sol/contract_RateLimitingConstraint.md]

## Metadata

- **Contract**: RateLimitingConstraint
- **Signature**: `constructor(address,address)`
- **Visibility**: public
- **Source Range**: 1929:185:42

## Implementation

```solidity
/// @notice Contract constructor
///  @param _activePoolObserver Address of the active pool observer
///  @param _governance Address of the governance mechanism
constructor(address _activePoolObserver, address _governance) {
    ACTIVE_POOL_OBSERVER = IActivePoolObserver(_activePoolObserver);
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

## State Variable Reads

- **_authority** (`contract Authority`) [src/Dependencies/Authority.sol/interface_Authority.md]
- **_authorityInitialized** (`bool`)

## State Variable Writes

- **ACTIVE_POOL_OBSERVER** (`contract IActivePoolObserver`) [src/Dependencies/IActivePoolObserver.sol/interface_IActivePoolObserver.md]
- **_authority** (`contract Authority`) [src/Dependencies/Authority.sol/interface_Authority.md]
- **_authorityInitialized** (`bool`)

## Call Tree

```
┌─ [0] 🏗️ CONSTRUCTOR: RateLimitingConstraint.constructor(address,address) (NodeID: 0)
    💬 Args: [no args]
    🏗️  Contract: RateLimitingConstraint
  └─ [1] ⚙️ FUNCTION: AuthNoOwner._initializeAuthority(address) (NodeID: 1)
      💬 Args: [_governance]
      👁️  Def: internal
```

## Documentation

### Function Documentation

@notice Contract constructor
 @param _activePoolObserver Address of the active pool observer
 @param _governance Address of the governance mechanism
