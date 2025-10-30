# Function: repayLoan(uint256,bool)

**Contract**: [src/TokenHolder.sol/contract_TokenHolder.md]

## Metadata

- **Contract**: TokenHolder
- **Signature**: `repayLoan(uint256,bool)`
- **Visibility**: public
- **Source Range**: 4912:2144:13

## Implementation

```solidity
function repayLoan(uint256 loanId, bool withoutTransfer) public onlyBorrowerRouter() {
    require(loanId < nextLoanId, "Invalid loan ID");
    Loan memory loanToRepay = loans[loanId];
    require(loanToRepay.amount > 0, "Loan does not exist or already repaid");
    address collateralAddress = loanToRepay.collateral.collateralAddress;
    uint256 loanDuration = block.timestamp - loanToRepay.timestamp;
    uint256 daysElapsed = ((loanDuration + 1 days) - 1) / 1 days;
    uint256 interestAmount = ((loanToRepay.amount * loanToRepay.collateral.interestRate) * daysElapsed) / (365 * 100);
    uint256 totalAmountDue = loanToRepay.amount + interestAmount;
    if (!withoutTransfer) {
        require(tokenHolded.transferFrom(msg.sender, address(this), totalAmountDue), "Token transfer failed");
    }
    if (collateralMapping[collateralAddress].currentExposure >= loanToRepay.amount) {
        collateralMapping[collateralAddress].currentExposure -= loanToRepay.amount;
        emit ExposureUpdated(collateralAddress, collateralMapping[collateralAddress].currentExposure);
    } else {
        collateralMapping[collateralAddress].currentExposure = 0;
        emit ExposureUpdated(collateralAddress, 0);
    }
    uint256 indexToRemove = loanIdToArrayIndex[loanId];
    uint256 lastIndex = activeLoanIds.length - 1;
    if (indexToRemove != lastIndex) {
        uint256 lastLoanId = activeLoanIds[lastIndex];
        activeLoanIds[indexToRemove] = lastLoanId;
        loanIdToArrayIndex[lastLoanId] = indexToRemove;
    }
    activeLoanIds.pop();
    delete loanIdToArrayIndex[loanId];
    delete loans[loanId];
    emit LoanRepaid(loanId);
}
```

## Related Implementations

### onlyBorrowerRouter()

- **Kind**: modifier
- **Source**: 2083:193:13
- **Link**: `src/TokenHolder.sol:TokenHolder:onlyBorrowerRouter()`

```solidity
modifier onlyBorrowerRouter() {
    require(hasRole(BORROWER_ROUTER_ROLE, msg.sender), "Only the borrower router can call this function");
    _;
}
```

### hasRole(bytes32,address)

- **Kind**: internal
- **Source**: 3801:207:0
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/access/AccessControlUpgradeable.sol:AccessControlUpgradeable:hasRole(bytes32,address)`

```solidity
///  @dev Returns `true` if `account` has been granted `role`.
function hasRole(bytes32 role, address account) virtual public view returns (bool) {
    AccessControlStorage storage $ = _getAccessControlStorage();
    return $._roles[role].hasRole[account];
}
```

### _getAccessControlStorage()

- **Kind**: internal
- **Source**: 2889:177:0
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/access/AccessControlUpgradeable.sol:AccessControlUpgradeable:_getAccessControlStorage()`

```solidity
function _getAccessControlStorage() private pure returns (AccessControlStorage storage $) {
    assembly {
        $.slot := AccessControlStorageLocation
    }
}
```

## External Calls

- **IERC20::transferFrom(address,address,uint256)**

## State Variable Reads

- **nextLoanId** (`uint256`)
- **loans** (`mapping(uint256 => struct Loan)`)
- **tokenHolded** (`contract IERC20`) [lib/openzeppelin-contracts-upgradeable/lib/openzeppelin-contracts/contracts/token/ERC20/IERC20.sol/interface_IERC20.md]
- **collateralMapping** (`mapping(address => struct Collateral)`)
- **loanIdToArrayIndex** (`mapping(uint256 => uint256)`)
- **activeLoanIds** (`uint256[]`)
- **BORROWER_ROUTER_ROLE** (`bytes32`)

## State Variable Writes

- **collateralMapping** (`mapping(address => struct Collateral)`)
- **activeLoanIds** (`uint256[]`)
- **loanIdToArrayIndex** (`mapping(uint256 => uint256)`)
- **loans** (`mapping(uint256 => struct Loan)`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: TokenHolder.repayLoan(uint256,bool) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
  └─ [1] 🔒 MODIFIER: TokenHolder.onlyBorrowerRouter() (NodeID: 1)
      💬 Args: [no args]
    └─ [2] ⚙️ FUNCTION: AccessControlUpgradeable.hasRole(bytes32,address) (NodeID: 2)
        💬 Args: [BORROWER_ROUTER_ROLE, msg.sender]
        👁️  Def: public
      └─ [3] ⚙️ FUNCTION: AccessControlUpgradeable._getAccessControlStorage() (NodeID: 3)
          💬 Args: [no args]
          👁️  Def: private
```
