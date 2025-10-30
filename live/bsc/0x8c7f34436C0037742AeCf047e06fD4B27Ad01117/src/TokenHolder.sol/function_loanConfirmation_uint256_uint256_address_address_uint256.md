# Function: loanConfirmation(uint256,uint256,address,address,uint256)

**Contract**: [src/TokenHolder.sol/contract_TokenHolder.md]

## Metadata

- **Contract**: TokenHolder
- **Signature**: `loanConfirmation(uint256,uint256,address,address,uint256)`
- **Visibility**: public
- **Source Range**: 3072:1777:13

## Implementation

```solidity
function loanConfirmation(uint256 amount, uint256 collateralAmount, address collateralAddress, address borrower, uint256 userPaid) public onlyBorrowerRouter() returns (uint256) {
    require(amount > 0, "Amount must be greater than 0");
    require(tokenHolded.balanceOf(address(this)) >= amount, "Insufficient funds in the contract");
    Collateral storage collateral = collateralMapping[collateralAddress];
    require(collateral.active, "Collateral not supported");
    require((collateral.currentExposure + amount) <= collateral.maxExposure, "Exposure limit exceeded for this collateral");
    require(((collateralAmount * collateral.maxLendPerToken) / (10 ** ERC20Upgradeable(collateralAddress).decimals())) >= amount, "Borrowed too much");
    collateral.currentExposure += amount;
    emit ExposureUpdated(collateralAddress, collateral.currentExposure);
    uint256 loanId = nextLoanId;
    nextLoanId++;
    loans[loanId] = Loan(loanId, amount, collateral, collateralAmount, block.timestamp, borrower, userPaid);
    loanIdToArrayIndex[loanId] = activeLoanIds.length;
    activeLoanIds.push(loanId);
    emit LoanCreated(loanId, amount, block.timestamp, false);
    return loanId;
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

- **IERC20::balanceOf(address)**
- **ERC20Upgradeable::decimals()**

## State Variable Reads

- **tokenHolded** (`contract IERC20`) [lib/openzeppelin-contracts-upgradeable/lib/openzeppelin-contracts/contracts/token/ERC20/IERC20.sol/interface_IERC20.md]
- **collateralMapping** (`mapping(address => struct Collateral)`)
- **nextLoanId** (`uint256`)
- **activeLoanIds** (`uint256[]`)
- **BORROWER_ROUTER_ROLE** (`bytes32`)

## State Variable Writes

- **nextLoanId** (`uint256`)
- **loans** (`mapping(uint256 => struct Loan)`)
- **loanIdToArrayIndex** (`mapping(uint256 => uint256)`)
- **activeLoanIds** (`uint256[]`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: TokenHolder.loanConfirmation(uint256,uint256,address,address,uint256) (NodeID: 0)
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
