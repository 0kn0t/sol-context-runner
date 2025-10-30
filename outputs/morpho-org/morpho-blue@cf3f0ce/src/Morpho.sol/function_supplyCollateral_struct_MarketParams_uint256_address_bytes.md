# Function: supplyCollateral(struct MarketParams,uint256,address,bytes)

**Contract**: [src/Morpho.sol/contract_Morpho.md]

## Metadata

- **Contract**: Morpho
- **Signature**: `supplyCollateral(struct MarketParams,uint256,address,bytes)`
- **Visibility**: external
- **Source Range**: 10937:804:0

## Implementation

```solidity
/// @inheritdoc IMorphoBase
function supplyCollateral(MarketParams memory marketParams, uint256 assets, address onBehalf, bytes calldata data) external {
    Id id = marketParams.id();
    require(market[id].lastUpdate != 0, ErrorsLib.MARKET_NOT_CREATED);
    require(assets != 0, ErrorsLib.ZERO_ASSETS);
    require(onBehalf != address(0), ErrorsLib.ZERO_ADDRESS);
    position[id][onBehalf].collateral += assets.toUint128();
    emit EventsLib.SupplyCollateral(id, msg.sender, onBehalf, assets);
    if (data.length > 0) IMorphoSupplyCollateralCallback(msg.sender).onMorphoSupplyCollateral(assets, data);
    IERC20(marketParams.collateralToken).safeTransferFrom(msg.sender, address(this), assets);
}
```

## Related Implementations

### id(struct MarketParams)

- **Kind**: internal
- **Source**: 598:222:9
- **Link**: `src/libraries/MarketParamsLib.sol:MarketParamsLib:id(struct MarketParams)`

```solidity
/// @notice Returns the id of the market `marketParams`.
function id(MarketParams memory marketParams) internal pure returns (Id marketParamsId) {
    assembly ("memory-safe") {
        marketParamsId := keccak256(marketParams, MARKET_PARAMS_BYTES_LENGTH)
    }
}
```

### toUint128(uint256)

- **Kind**: internal
- **Source**: 826:169:13
- **Link**: `src/libraries/UtilsLib.sol:UtilsLib:toUint128(uint256)`

```solidity
/// @dev Returns `x` safely cast to uint128.
function toUint128(uint256 x) internal pure returns (uint128) {
    require(x <= type(uint128).max, ErrorsLib.MAX_UINT128_EXCEEDED);
    return uint128(x);
}
```

## External Calls

- **IMorphoSupplyCollateralCallback::onMorphoSupplyCollateral(uint256,bytes)**
- **IERC20::safeTransferFrom(contract IERC20,address,address,uint256)**

## State Variable Reads

- **market** (`mapping(Id => struct Market)`)

## State Variable Writes

- **position** (`mapping(Id => mapping(address => struct Position))`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: Morpho.supplyCollateral(struct MarketParams,uint256,address,bytes) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: external
  ├─ [1] ⚙️ FUNCTION: MarketParamsLib.id(struct MarketParams) (NodeID: 1)
  │   💬 Args: [marketParams]
  │   👁️  Def: internal
  └─ [1] ⚙️ FUNCTION: UtilsLib.toUint128(uint256) (NodeID: 2)
      💬 Args: [assets]
      👁️  Def: internal
```

## Documentation

### Function Documentation

@inheritdoc IMorphoBase

### Interface Documentation

@notice Supplies `assets` of collateral on behalf of `onBehalf`, optionally calling back the caller's
 `onMorphoSupplyCollateral` function with the given `data`.
 @dev Interest are not accrued since it's not required and it saves gas.
 @dev Supplying a large amount can revert for overflow.
 @param marketParams The market to supply collateral to.
 @param assets The amount of collateral to supply.
 @param onBehalf The address that will own the increased collateral position.
 @param data Arbitrary data to pass to the `onMorphoSupplyCollateral` callback. Pass empty data if not needed.
