# Function: setMaxApprovalAmount(uint256)

**Contract**: [src/BorrowerOperationsV6.sol/contract_BorrowerOperationsV6.md]

## Metadata

- **Contract**: BorrowerOperationsV6
- **Signature**: `setMaxApprovalAmount(uint256)`
- **Visibility**: external
- **Source Range**: 3752:336:12

## Implementation

```solidity
function setMaxApprovalAmount(uint256 _maxApprovalAmount) external onlyAdmin() {
    require(_maxApprovalAmount > 0, "Max approval amount must be greater than 0");
    uint256 oldAmount = maxApprovalAmount;
    maxApprovalAmount = _maxApprovalAmount;
    emit MaxApprovalAmountUpdated(oldAmount, _maxApprovalAmount);
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

- **maxApprovalAmount** (`uint256`)
- **admin** (`address`)

## State Variable Writes

- **maxApprovalAmount** (`uint256`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: BorrowerOperationsV6.setMaxApprovalAmount(uint256) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: external
  └─ [1] 🔒 MODIFIER: BorrowerOperationsV6.onlyAdmin() (NodeID: 1)
      💬 Args: [no args]
```
