# Function: claimProfit()

**Contract**: [src/ERC4626Escrow.sol/contract_ERC4626Escrow.md]

## Metadata

- **Contract**: ERC4626Escrow
- **Signature**: `claimProfit()`
- **Visibility**: external
- **Source Range**: 6312:76:22
- **Inherited From**: BaseEscrow

## Implementation

```solidity
/// @notice Claim profit (fees + external lending profit)
///  @dev can only be called by authorized users
function claimProfit() external requiresAuth() {
    _claimProfit();
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

### requiresAuth()

- **Kind**: modifier
- **Source**: 647:125:25
- **Link**: `src/Dependencies/AuthNoOwner.sol:AuthNoOwner:requiresAuth()`

```solidity
modifier requiresAuth() virtual {
    require(isAuthorized(msg.sender, msg.sig), "Auth: UNAUTHORIZED");
    _;
}
```

### isAuthorized(address,bytes4)

- **Kind**: internal
- **Source**: 981:524:25
- **Link**: `src/Dependencies/AuthNoOwner.sol:AuthNoOwner:isAuthorized(address,bytes4)`

```solidity
function isAuthorized(address user, bytes4 functionSig) virtual internal view returns (bool) {
    Authority auth = _authority;
    return ((address(auth) != address(0)) && auth.canCall(user, address(this), functionSig));
}
```

## State Variable Reads

- **totalAssetsDeposited** (`uint256`)
- **EXTERNAL_VAULT** (`contract IERC4626`) [lib/openzeppelin-contracts/contracts/interfaces/IERC4626.sol/interface_IERC4626.md]
- **ASSET_TOKEN** (`contract IERC20`) [lib/openzeppelin-contracts/contracts/token/ERC20/IERC20.sol/interface_IERC20.md]
- **FEE_RECIPIENT** (`address`)
- **_authority** (`contract Authority`) [src/Dependencies/Authority.sol/interface_Authority.md]

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: BaseEscrow.claimProfit() (NodeID: 0)
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
  └─ [1] 🔒 MODIFIER: AuthNoOwner.requiresAuth() (NodeID: 12)
      💬 Args: [no args]
    └─ [2] ⚙️ FUNCTION: AuthNoOwner.isAuthorized(address,bytes4) (NodeID: 13)
        💬 Args: [msg.sender, msg.sig]
        👁️  Def: internal
```

## Documentation

### Function Documentation

@notice Claim profit (fees + external lending profit)
 @dev can only be called by authorized users
