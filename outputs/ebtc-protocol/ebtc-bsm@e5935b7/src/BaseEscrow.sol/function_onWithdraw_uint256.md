# Function: onWithdraw(uint256)

**Contract**: [src/BaseEscrow.sol/contract_BaseEscrow.md]

## Metadata

- **Contract**: BaseEscrow
- **Signature**: `onWithdraw(uint256)`
- **Visibility**: external
- **Source Range**: 4472:126:22

## Implementation

```solidity
/// @notice Withdraws assets from the escrow
///  @dev Can only be called by the bsm contract
///  @param _assetAmount The amount of assets to withdraw
///  @return The amount of assets withdrawn
function onWithdraw(uint256 _assetAmount) external onlyBSM() returns (uint256) {
    return _onWithdraw(_assetAmount);
}
```

## Related Implementations

### _onWithdraw(uint256)

- **Kind**: internal
- **Source**: 2253:316:22
- **Link**: `src/BaseEscrow.sol:BaseEscrow:_onWithdraw(uint256)`

```solidity
/// @notice Internal function to handle asset withdrawals
///  @param _assetAmount The amount of assets to withdraw
///  @return The amount of assets actually withdrawn
function _onWithdraw(uint256 _assetAmount) virtual internal returns (uint256) {
    totalAssetsDeposited -= _assetAmount;
    /// @dev returning the amount requested since this is the base contract
    ///  it's possible for other implementations to return lower amounts
    return _assetAmount;
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
┌─ [0] ⚙️ FUNCTION: BaseEscrow.onWithdraw(uint256) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: external
  ├─ [1] ⚙️ FUNCTION: BaseEscrow._onWithdraw(uint256) (NodeID: 1)
  │   💬 Args: [_assetAmount]
  │   👁️  Def: internal
  └─ [1] 🔒 MODIFIER: BaseEscrow.onlyBSM() (NodeID: 2)
      💬 Args: [no args]
```

## Documentation

### Function Documentation

@notice Withdraws assets from the escrow
 @dev Can only be called by the bsm contract
 @param _assetAmount The amount of assets to withdraw
 @return The amount of assets withdrawn
