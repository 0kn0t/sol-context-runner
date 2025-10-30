# Function: onMigrateSource(address)

**Contract**: [src/BaseEscrow.sol/contract_BaseEscrow.md]

## Metadata

- **Contract**: BaseEscrow
- **Signature**: `onMigrateSource(address)`
- **Visibility**: external
- **Source Range**: 5028:519:22

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
- **Source**: 3278:74:22
- **Link**: `src/BaseEscrow.sol:BaseEscrow:_beforeMigration()`

```solidity
/// @notice Prepares the escrow for migration
function _beforeMigration() virtual internal {}
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
  │ │ └─ [3] ⚙️ FUNCTION: BaseEscrow._totalBalance() (NodeID: 3)
  │ │     💬 Args: [no args]
  │ │     👁️  Def: internal
  │ ├─ [2] ⚙️ FUNCTION: BaseEscrow._withdrawProfit(uint256) (NodeID: 4)
  │ │   💬 Args: [profit]
  │ │   👁️  Def: internal
  │ └─ [2] ⚙️ FUNCTION: BaseEscrow._totalBalance() (NodeID: 5)
  │     💬 Args: [no args]
  │     👁️  Def: internal
  ├─ [1] ⚙️ FUNCTION: BaseEscrow._beforeMigration() (NodeID: 6)
  │   💬 Args: [no args]
  │   👁️  Def: internal
  └─ [1] 🔒 MODIFIER: BaseEscrow.onlyBSM() (NodeID: 7)
      💬 Args: [no args]
```

## Documentation

### Function Documentation

@notice Called on the source escrow during a migration by the BSM to transfer liquidity
 @dev Can only be called by the bsm contract
 @param _newEscrow new escrow address
