# Function: claimProfit()

**Contract**: [src/BaseEscrow.sol/contract_BaseEscrow.md]

## Metadata

- **Contract**: BaseEscrow
- **Signature**: `claimProfit()`
- **Visibility**: external
- **Source Range**: 6312:76:22

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
  │ │ └─ [3] ⚙️ FUNCTION: BaseEscrow._totalBalance() (NodeID: 3)
  │ │     💬 Args: [no args]
  │ │     👁️  Def: internal
  │ ├─ [2] ⚙️ FUNCTION: BaseEscrow._withdrawProfit(uint256) (NodeID: 4)
  │ │   💬 Args: [profit]
  │ │   👁️  Def: internal
  │ └─ [2] ⚙️ FUNCTION: BaseEscrow._totalBalance() (NodeID: 5)
  │     💬 Args: [no args]
  │     👁️  Def: internal
  └─ [1] 🔒 MODIFIER: AuthNoOwner.requiresAuth() (NodeID: 6)
      💬 Args: [no args]
    └─ [2] ⚙️ FUNCTION: AuthNoOwner.isAuthorized(address,bytes4) (NodeID: 7)
        💬 Args: [msg.sender, msg.sig]
        👁️  Def: internal
```

## Documentation

### Function Documentation

@notice Claim profit (fees + external lending profit)
 @dev can only be called by authorized users
