# Function: onMigrateSource(address)

**Contract**: [src/ERC4626Escrow.sol/contract_ERC4626Escrow.md]

## Metadata

- **Contract**: ERC4626Escrow
- **Signature**: `onMigrateSource(address)`
- **Visibility**: external
- **Source Range**: 5028:519:22
- **Inherited From**: BaseEscrow

## Implementation

```solidity
/// @notice Called on the source escrow during a migration by the BSM to transfer liquidity
///  @dev Can only be called by the bsm contract
///  @param _newEscrow new escrow address
function onMigrateSource(address _newEscrow) external onlyBSM() {
    /// @dev take profit first (totalBalance == depositAmount after)
    _claimProfit();
    /// @dev clear depositAmount in old vault (address(this))
    totalAssetsDeposited = 0;
    /// @dev perform pre-migration tasks (potentially used by derived contracts)
    _beforeMigration();
    /// @dev transfer all liquidity to new vault
    ASSET_TOKEN.safeTransfer(_newEscrow, ASSET_TOKEN.balanceOf(address(this)));
}
```

## Related Implementations

### _claimProfit()

- **Kind**: internal
- **Source**: 3450:375:22
- **Link**: `src/BaseEscrow.sol:BaseEscrow:_claimProfit()`

```solidity
/// @notice Internal function to claim profits generated from fees and external lending
function _claimProfit() internal {
    uint256 profit = feeProfit();
    if (profit > 0) {
        uint256 profitWithdrawn = _withdrawProfit(profit);
        require(_totalBalance() >= totalAssetsDeposited, LossCheck());
        emit ProfitClaimed(profitWithdrawn);
    }
}
```

### feeProfit()

- **Kind**: internal
- **Source**: 5939:253:22
- **Link**: `src/BaseEscrow.sol:BaseEscrow:feeProfit()`

```solidity
/// @notice Calculates the profit generated from asset management
///  @return The amount of profit generated
function feeProfit() public view returns (uint256) {
    uint256 tb = _totalBalance();
    if (tb > totalAssetsDeposited) {
        unchecked {
            return tb - totalAssetsDeposited;
        }
    }
    return 0;
}
```

### _totalBalance()

- **Kind**: internal
- **Source**: 4995:275:39
- **Link**: `src/ERC4626Escrow.sol:ERC4626Escrow:_totalBalance()`

```solidity
/// @notice Returns the total balance of assets
///  @dev Combines local balance with the balance held in the external vault
///  @return The total balance of assets
function _totalBalance() override internal view returns (uint256) {
    /// @dev convertToAssets is the same as previewRedeem for OZ, Aave, Euler, Morpho
    return EXTERNAL_VAULT.convertToAssets(EXTERNAL_VAULT.balanceOf(address(this))) + super._totalBalance();
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

### _withdrawProfit(uint256)

- **Kind**: internal
- **Source**: 5440:211:39
- **Link**: `src/ERC4626Escrow.sol:ERC4626Escrow:_withdrawProfit(uint256)`

```solidity
/// @notice Overrides _withdrawProfit from BaseEscrow to manage liquidity from the external vault
///  @param _profitAmount The amount of profit to withdraw
function _withdrawProfit(uint256 _profitAmount) override internal returns (uint256) {
    uint256 redeemedAmount = _ensureLiquidity(_profitAmount);
    return super._withdrawProfit(redeemedAmount);
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

### _withdrawProfit(uint256)

- **Kind**: internal
- **Source**: 3039:183:22
- **Link**: `src/BaseEscrow.sol:BaseEscrow:_withdrawProfit(uint256)`

```solidity
/// @notice withdraw profit to FEE_RECIPIENT
///  @param _profitAmount The amount of profit to withdraw
function _withdrawProfit(uint256 _profitAmount) virtual internal returns (uint256) {
    ASSET_TOKEN.safeTransfer(FEE_RECIPIENT, _profitAmount);
    return _profitAmount;
}
```

### _beforeMigration()

- **Kind**: internal
- **Source**: 7171:228:39
- **Link**: `src/ERC4626Escrow.sol:ERC4626Escrow:_beforeMigration()`

```solidity
/// @notice Prepares the contract for migration by redeeming all shares from the external vault
///  @notice We expect to redeem here only if the escrow balance is low. For large escrow balances,
///  we will call `redeemFromExternalVault` before a migration to have proper slippage checks.
function _beforeMigration() override internal {
    uint256 shares = EXTERNAL_VAULT.balanceOf(address(this));
    if (shares > 0) {
        EXTERNAL_VAULT.redeem(shares, address(this), address(this));
    }
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

## External Calls

- **IERC20::safeTransfer(contract IERC20,address,uint256)**
- **IERC20::balanceOf(address)**

## State Variable Reads

- **ASSET_TOKEN** (`contract IERC20`) [lib/openzeppelin-contracts/contracts/token/ERC20/IERC20.sol/interface_IERC20.md]
- **totalAssetsDeposited** (`uint256`)
- **EXTERNAL_VAULT** (`contract IERC4626`) [lib/openzeppelin-contracts/contracts/interfaces/IERC4626.sol/interface_IERC4626.md]
- **FEE_RECIPIENT** (`address`)
- **BSM** (`address`)

## State Variable Writes

- **totalAssetsDeposited** (`uint256`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: BaseEscrow.onMigrateSource(address) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: external
  ├─ [1] ⚙️ FUNCTION: BaseEscrow._claimProfit() (NodeID: 1)
  │   💬 Args: [no args]
  │   👁️  Def: internal
  │ ├─ [2] ⚙️ FUNCTION: BaseEscrow.feeProfit() (NodeID: 2)
  │ │   💬 Args: [no args]
  │ │   👁️  Def: public
  │ │ └─ [3] ⚙️ FUNCTION: ERC4626Escrow._totalBalance() (NodeID: 3)
  │ │     💬 Args: [no args]
  │ │     👁️  Def: internal
  │ │   └─ [4] ⚙️ FUNCTION: BaseEscrow._totalBalance() (NodeID: 4)
  │ │       💬 Args: [no args]
  │ │       👁️  Def: internal
  │ ├─ [2] ⚙️ FUNCTION: ERC4626Escrow._withdrawProfit(uint256) (NodeID: 5)
  │ │   💬 Args: [profit]
  │ │   👁️  Def: internal
  │ │ ├─ [3] ⚙️ FUNCTION: ERC4626Escrow._ensureLiquidity(uint256) (NodeID: 6)
  │ │ │   💬 Args: [_profitAmount]
  │ │ │   👁️  Def: private
  │ │ │ ├─ [4] ⚙️ FUNCTION: BaseEscrow._totalBalance() (NodeID: 7)
  │ │ │ │   💬 Args: [no args]
  │ │ │ │   👁️  Def: internal
  │ │ │ └─ [4] ⚙️ FUNCTION: ERC4626Escrow._clampShares(uint256) (NodeID: 8)
  │ │ │     💬 Args: [EXTERNAL_VAULT.convertToShares(deficit)]
  │ │ │     👁️  Def: internal
  │ │ └─ [3] ⚙️ FUNCTION: BaseEscrow._withdrawProfit(uint256) (NodeID: 9)
  │ │     💬 Args: [redeemedAmount]
  │ │     👁️  Def: internal
  │ └─ [2] ⚙️ FUNCTION: ERC4626Escrow._totalBalance() (NodeID: 10)
  │     💬 Args: [no args]
  │     👁️  Def: internal
  │   └─ [3] ⚙️ FUNCTION: BaseEscrow._totalBalance() (NodeID: 11)
  │       💬 Args: [no args]
  │       👁️  Def: internal
  ├─ [1] ⚙️ FUNCTION: ERC4626Escrow._beforeMigration() (NodeID: 12)
  │   💬 Args: [no args]
  │   👁️  Def: internal
  └─ [1] 🔒 MODIFIER: BaseEscrow.onlyBSM() (NodeID: 13)
      💬 Args: [no args]
```

## Documentation

### Function Documentation

@notice Called on the source escrow during a migration by the BSM to transfer liquidity
 @dev Can only be called by the bsm contract
 @param _newEscrow new escrow address
