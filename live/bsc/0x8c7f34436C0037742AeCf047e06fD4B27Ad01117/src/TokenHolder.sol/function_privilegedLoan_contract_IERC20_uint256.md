# Function: privilegedLoan(contract IERC20,uint256)

**Contract**: [src/TokenHolder.sol/contract_TokenHolder.md]

## Metadata

- **Contract**: TokenHolder
- **Signature**: `privilegedLoan(contract IERC20,uint256)`
- **Visibility**: public
- **Source Range**: 8886:492:13

## Implementation

```solidity
function privilegedLoan(IERC20 flashLoanToken, uint256 amount) public onlyBorrowerRouter() {
    require(amount > 0, "Amount must be greater than 0");
    require(flashLoanToken.balanceOf(address(this)) >= amount, "Insufficient funds in the contract");
    require(flashLoanToken.transfer(msg.sender, amount), "Token transfer failed");
    emit PrivilegedLoan(msg.sender, amount);
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
- **IERC20::transfer(address,uint256)**

## Native Transfers

- **flashLoanToken** (function parameter)

## State Variable Reads

- **BORROWER_ROUTER_ROLE** (`bytes32`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: TokenHolder.privilegedLoan(contract IERC20,uint256) (NodeID: 0)
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
