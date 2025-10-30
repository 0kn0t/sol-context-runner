# Function: totalBalance()

**Contract**: [src/BaseEscrow.sol/contract_BaseEscrow.md]

## Metadata

- **Contract**: BaseEscrow
- **Signature**: `totalBalance()`
- **Visibility**: external
- **Source Range**: 3897:95:22

## Implementation

```solidity
/// @notice Returns the total balance of assets in the escrow
function totalBalance() external view returns (uint256) {
    return _totalBalance();
}
```

## Related Implementations

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

## State Variable Reads

- **ASSET_TOKEN** (`contract IERC20`) [lib/openzeppelin-contracts/contracts/token/ERC20/IERC20.sol/interface_IERC20.md]

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: BaseEscrow.totalBalance() (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: external
  └─ [1] ⚙️ FUNCTION: BaseEscrow._totalBalance() (NodeID: 1)
      💬 Args: [no args]
      👁️  Def: internal
```

## Documentation

### Function Documentation

@notice Returns the total balance of assets in the escrow
