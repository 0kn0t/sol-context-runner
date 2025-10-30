# Function: canProcess(uint256,address)

**Contract**: [src/RateLimitingConstraint.sol/contract_RateLimitingConstraint.md]

## Metadata

- **Contract**: RateLimitingConstraint
- **Signature**: `canProcess(uint256,address)`
- **Visibility**: external
- **Source Range**: 2445:765:42

## Implementation

```solidity
/// @notice Checks if the minting amount is within the allowed cap for the minter
///  @param _amount The amount to be minted
///  @param _bsm The address of the minter
///  @return bool True if the minting is within the cap, false otherwise
///  @return bytes Encoded error data if the mint is above the cap
function canProcess(uint256 _amount, address _bsm) external view returns (bool, bytes memory) {
    MintingConfig memory cap = mintingConfig[_bsm];
    uint256 maxMint;
    if (cap.useAbsoluteCap) {
        maxMint = cap.absoluteCap;
    } else {
        /// @notice ACTIVE_POOL.observe returns the eBTC TWAP total supply
        uint256 totalEbtcSupply = ACTIVE_POOL_OBSERVER.observe();
        maxMint = (totalEbtcSupply * cap.relativeCapBPS) / BPS;
    }
    uint256 newTotalToMint = IEbtcBSM(_bsm).totalMinted() + _amount;
    if (newTotalToMint > maxMint) {
        return (false, abi.encodeWithSelector(AboveMintingCap.selector, _amount, newTotalToMint, maxMint));
    }
    return (true, "");
}
```

## External Calls

- **IActivePoolObserver::observe()**
- **IEbtcBSM::totalMinted()**

## State Variable Reads

- **mintingConfig** (`mapping(address => struct RateLimitingConstraint.MintingConfig)`)
- **ACTIVE_POOL_OBSERVER** (`contract IActivePoolObserver`) [src/Dependencies/IActivePoolObserver.sol/interface_IActivePoolObserver.md]
- **BPS** (`uint256`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: RateLimitingConstraint.canProcess(uint256,address) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: external
```

## Documentation

### Function Documentation

@notice Checks if the minting amount is within the allowed cap for the minter
 @param _amount The amount to be minted
 @param _bsm The address of the minter
 @return bool True if the minting is within the cap, false otherwise
 @return bytes Encoded error data if the mint is above the cap
