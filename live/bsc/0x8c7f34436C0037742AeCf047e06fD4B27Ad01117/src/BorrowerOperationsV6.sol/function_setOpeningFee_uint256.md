# Function: setOpeningFee(uint256)

**Contract**: [src/BorrowerOperationsV6.sol/contract_BorrowerOperationsV6.md]

## Metadata

- **Contract**: BorrowerOperationsV6
- **Signature**: `setOpeningFee(uint256)`
- **Visibility**: external
- **Source Range**: 2459:104:12

## Implementation

```solidity
function setOpeningFee(uint256 _openingFee) external onlyAdmin() {
    openingFee = _openingFee;
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

- **openingFee** (`uint256`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: BorrowerOperationsV6.setOpeningFee(uint256) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: external
  └─ [1] 🔒 MODIFIER: BorrowerOperationsV6.onlyAdmin() (NodeID: 1)
      💬 Args: [no args]
```
