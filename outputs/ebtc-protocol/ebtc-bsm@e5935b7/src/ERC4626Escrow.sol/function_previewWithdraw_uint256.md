# Function: previewWithdraw(uint256)

**Contract**: [src/ERC4626Escrow.sol/contract_ERC4626Escrow.md]

## Metadata

- **Contract**: ERC4626Escrow
- **Signature**: `previewWithdraw(uint256)`
- **Visibility**: external
- **Source Range**: 4604:225:22
- **Inherited From**: BaseEscrow

## Implementation

```solidity
function previewWithdraw(uint256 _assetAmount) external view returns (uint256) {
    /// @dev derived contracts can override _previewWithdraw to report potential losses
    return _previewWithdraw(_assetAmount);
}
```

## Related Implementations

### _previewWithdraw(uint256)

- **Kind**: internal
- **Source**: 5890:974:39
- **Link**: `src/ERC4626Escrow.sol:ERC4626Escrow:_previewWithdraw(uint256)`

```solidity
/// @notice Preview the amount of assets that would be withdrawn for a given amount of shares
///  @param _assetAmount The amount of assets for which to preview the withdrawal
///  @return The previewed withdrawable amount
function _previewWithdraw(uint256 _assetAmount) override internal view returns (uint256) {
    uint256 liquidBalance = super._totalBalance();
    if (_assetAmount > liquidBalance) {
        uint256 deficit;
        unchecked {
            deficit = _assetAmount - liquidBalance;
        }
        /// @dev using convertToShares + previewRedeem instead of previewWithdraw to round down
        uint256 shares = _clampShares(EXTERNAL_VAULT.convertToShares(deficit));
        /// Cap for edge case when `previewRedeem` returns more assets than `convertToShares`
        uint256 redeemed;
        if (shares > 0) {
            redeemed = EXTERNAL_VAULT.previewRedeem(shares);
            if (redeemed > deficit) {
                redeemed = deficit;
            }
        }
        return liquidBalance + redeemed;
    } else {
        return _assetAmount;
    }
}
```

### _totalBalance()

- **Kind**: internal
- **Source**: 1706:125:22
- **Link**: `src/BaseEscrow.sol:BaseEscrow:_totalBalance()`

```solidity
/// @dev Internal function that checks the total balance of assets held by the contract
function _totalBalance() virtual internal view returns (uint256) {
    return ASSET_TOKEN.balanceOf(address(this));
}
```

### _clampShares(uint256)

- **Kind**: internal
- **Source**: 2513:252:39
- **Link**: `src/ERC4626Escrow.sol:ERC4626Escrow:_clampShares(uint256)`

```solidity
/// @notice make sure we are not trying to redeem more shares than we have
function _clampShares(uint256 _shares) internal view returns (uint256) {
    uint256 totalShares = EXTERNAL_VAULT.balanceOf(address(this));
    if (_shares > totalShares) {
        return totalShares;
    }
    return _shares;
}
```

## State Variable Reads

- **EXTERNAL_VAULT** (`contract IERC4626`) [lib/openzeppelin-contracts/contracts/interfaces/IERC4626.sol/interface_IERC4626.md]
- **ASSET_TOKEN** (`contract IERC20`) [lib/openzeppelin-contracts/contracts/token/ERC20/IERC20.sol/interface_IERC20.md]

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: BaseEscrow.previewWithdraw(uint256) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: external
  └─ [1] ⚙️ FUNCTION: ERC4626Escrow._previewWithdraw(uint256) (NodeID: 1)
      💬 Args: [_assetAmount]
      👁️  Def: internal
    ├─ [2] ⚙️ FUNCTION: BaseEscrow._totalBalance() (NodeID: 2)
    │   💬 Args: [no args]
    │   👁️  Def: internal
    └─ [2] ⚙️ FUNCTION: ERC4626Escrow._clampShares(uint256) (NodeID: 3)
        💬 Args: [EXTERNAL_VAULT.convertToShares(deficit)]
        👁️  Def: internal
```
