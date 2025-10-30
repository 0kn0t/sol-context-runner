# Function: onDeposit(uint256)

**Contract**: [src/BaseEscrow.sol/contract_BaseEscrow.md]

## Metadata

- **Contract**: BaseEscrow
- **Signature**: `onDeposit(uint256)`
- **Visibility**: external
- **Source Range**: 4158:99:22

## Implementation

```solidity
/// @notice Deposits assets into the escrow
///  @dev Can only be called by the bsm contract
///  @param _assetAmount The amount of assets to deposit
function onDeposit(uint256 _assetAmount) external onlyBSM() {
    _onDeposit(_assetAmount);
}
```

## Related Implementations

### _onDeposit(uint256)

- **Kind**: internal
- **Source**: 1956:112:22
- **Link**: `src/BaseEscrow.sol:BaseEscrow:_onDeposit(uint256)`

```solidity
/// @notice Internal function to handle asset deposits
///  @param _assetAmount The amount of assets to deposit
function _onDeposit(uint256 _assetAmount) virtual internal {
    totalAssetsDeposited += _assetAmount;
}
```

### onlyBSM()

- **Kind**: modifier
- **Source**: 787:115:22
- **Link**: `src/BaseEscrow.sol:BaseEscrow:onlyBSM()`

```solidity
/// @notice Modifier to restrict function calls to the BSM
modifier onlyBSM() {
    if (msg.sender != BSM) {
        revert CallerNotBSM();
    }
    _;
}
```

## State Variable Reads

- **BSM** (`address`)

## State Variable Writes

- **totalAssetsDeposited** (`uint256`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: BaseEscrow.onDeposit(uint256) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: external
  ├─ [1] ⚙️ FUNCTION: BaseEscrow._onDeposit(uint256) (NodeID: 1)
  │   💬 Args: [_assetAmount]
  │   👁️  Def: internal
  └─ [1] 🔒 MODIFIER: BaseEscrow.onlyBSM() (NodeID: 2)
      💬 Args: [no args]
```

## Documentation

### Function Documentation

@notice Deposits assets into the escrow
 @dev Can only be called by the bsm contract
 @param _assetAmount The amount of assets to deposit
