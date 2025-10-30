# Function: setProfitFee(uint256)

**Contract**: [src/BorrowerOperationsV6.sol/contract_BorrowerOperationsV6.md]

## Metadata

- **Contract**: BorrowerOperationsV6
- **Signature**: `setProfitFee(uint256)`
- **Visibility**: external
- **Source Range**: 2569:100:12

## Implementation

```solidity
function setProfitFee(uint256 _profitFee) external onlyAdmin() {
    profitFee = _profitFee;
}
```

## Related Implementations

### onlyAdmin()

- **Kind**: modifier
- **Source**: 2249:114:12
- **Link**: `src/BorrowerOperationsV6.sol:BorrowerOperationsV6:onlyAdmin()`

```solidity
modifier onlyAdmin() {
    require(msg.sender == admin, "Only admin can call this function");
    _;
}
```

## State Variable Reads

- **admin** (`address`)

## State Variable Writes

- **profitFee** (`uint256`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: BorrowerOperationsV6.setProfitFee(uint256) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: external
  └─ [1] 🔒 MODIFIER: BorrowerOperationsV6.onlyAdmin() (NodeID: 1)
      💬 Args: [no args]
```
