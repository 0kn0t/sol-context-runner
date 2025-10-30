# Contract: WadRayMath

## Metadata

- **Name**: WadRayMath
- **Type**: Contract
- **Path**: lib/aave-v3-origin/src/contracts/protocol/libraries/math/WadRayMath.sol
- **Documentation**:  @title WadRayMath library
   @author Aave
   @notice Provides functions to perform calculations with Wad and Ray units
   @dev Provides mul and div function for wads (decimal numbers with 18 digits of precision) and rays (decimal numbers
   with 27 digits of precision)
   @dev Operations are rounded. If a value is >=.5, will be rounded up, otherwise rounded down.

## State Variables

### WAD

```solidity
uint256 internal constant WAD = 1e18
```

### HALF_WAD

```solidity
uint256 internal constant HALF_WAD = 0.5e18
```

### RAY

```solidity
uint256 internal constant RAY = 1e27
```

### HALF_RAY

```solidity
uint256 internal constant HALF_RAY = 0.5e27
```

### WAD_RAY_RATIO

```solidity
uint256 internal constant WAD_RAY_RATIO = 1e9
```
