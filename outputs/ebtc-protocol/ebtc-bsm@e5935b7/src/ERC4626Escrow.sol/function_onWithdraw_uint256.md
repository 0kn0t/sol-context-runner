# Function: onWithdraw(uint256)

**Contract**: [src/ERC4626Escrow.sol/contract_ERC4626Escrow.md]

## Metadata

- **Contract**: ERC4626Escrow
- **Signature**: `onWithdraw(uint256)`
- **Visibility**: external
- **Source Range**: 4472:126:22
- **Inherited From**: BaseEscrow

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
- **Source**: 2105:323:39
- **Link**: `src/ERC4626Escrow.sol:ERC4626Escrow:_onWithdraw(uint256)`

```solidity
/// @notice Overrides _onWithdraw from BaseEscrow to manage liquidity from the external vault
///  @param _assetAmount The amount of assets to withdraw
///  @return redeemedAmount The actual amount of assets redeemed from the vault
function _onWithdraw(uint256 _assetAmount) override internal returns (uint256) {
    uint256 redeemedAmount = _ensureLiquidity(_assetAmount);
    super._onWithdraw(_assetAmount);
    return redeemedAmount;
}
```

### _ensureLiquidity(uint256)

- **Kind**: internal
- **Source**: 3016:1797:39
- **Link**: `src/ERC4626Escrow.sol:ERC4626Escrow:_ensureLiquidity(uint256)`

```solidity
/// @notice Ensures sufficient liquidity is available by redeeming assets from the external vault if necessary
///  @param _amountRequired The amount of assets required
///  @return _amountRedeemed The actual amount of assets redeemed
function _ensureLiquidity(uint256 _amountRequired) private returns (uint256 _amountRedeemed) {
    /// @dev super._totalBalance() returns asset balance for this contract
    uint256 liquidBalance = super._totalBalance();
    if (_amountRequired > liquidBalance) {
        uint256 deficit;
        unchecked {
            deficit = _amountRequired - liquidBalance;
        }
        /// @notice We assume convertToShares always returns less shares than previewWithdraw
        ///  @notice This may not be the case for Euler earn
        uint256 shares = _clampShares(EXTERNAL_VAULT.convertToShares(deficit));
        uint256 redeemed;
        /// @dev avoid redeeming 0 shares
        if (shares > 0) {
            uint256 balanceBefore = ASSET_TOKEN.balanceOf(address(this));
            EXTERNAL_VAULT.redeem(shares, address(this), address(this));
            uint256 balanceAfter = ASSET_TOKEN.balanceOf(address(this));
            redeemed = balanceAfter - balanceBefore;
        }
        _amountRedeemed = liquidBalance + redeemed;
        /// @audit Some vaults (most OOS) do not accrue their assets in `convertToShares`
        ///  This can cause `redeem` to withdraw more than requested
        ///  To prevent abuse, we cap at deficit
        if (redeemed > deficit) {
            _amountRedeemed = liquidBalance + deficit;
        }
    } else {
        _amountRedeemed = _amountRequired;
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

- **EXTERNAL_VAULT** (`contract IERC4626`) [lib/openzeppelin-contracts/contracts/interfaces/IERC4626.sol/interface_IERC4626.md]
- **ASSET_TOKEN** (`contract IERC20`) [lib/openzeppelin-contracts/contracts/token/ERC20/IERC20.sol/interface_IERC20.md]
- **BSM** (`address`)

## State Variable Writes

- **totalAssetsDeposited** (`uint256`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: BaseEscrow.onWithdraw(uint256) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: external
  ├─ [1] ⚙️ FUNCTION: ERC4626Escrow._onWithdraw(uint256) (NodeID: 1)
  │   💬 Args: [_assetAmount]
  │   👁️  Def: internal
  │ ├─ [2] ⚙️ FUNCTION: ERC4626Escrow._ensureLiquidity(uint256) (NodeID: 2)
  │ │   💬 Args: [_assetAmount]
  │ │   👁️  Def: private
  │ │ ├─ [3] ⚙️ FUNCTION: BaseEscrow._totalBalance() (NodeID: 3)
  │ │ │   💬 Args: [no args]
  │ │ │   👁️  Def: internal
  │ │ └─ [3] ⚙️ FUNCTION: ERC4626Escrow._clampShares(uint256) (NodeID: 4)
  │ │     💬 Args: [EXTERNAL_VAULT.convertToShares(deficit)]
  │ │     👁️  Def: internal
  │ └─ [2] ⚙️ FUNCTION: BaseEscrow._onWithdraw(uint256) (NodeID: 5)
  │     💬 Args: [_assetAmount]
  │     👁️  Def: internal
  └─ [1] 🔒 MODIFIER: BaseEscrow.onlyBSM() (NodeID: 6)
      💬 Args: [no args]
```

## Documentation

### Function Documentation

@notice Withdraws assets from the escrow
 @dev Can only be called by the bsm contract
 @param _assetAmount The amount of assets to withdraw
 @return The amount of assets withdrawn
