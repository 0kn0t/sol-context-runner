# Function: buy(bytes,contract IERC20,uint256,contract TokenHolder,address,address,address)

**Contract**: [src/BorrowerOperationsV6.sol/contract_BorrowerOperationsV6.md]

## Metadata

- **Contract**: BorrowerOperationsV6
- **Signature**: `buy(bytes,contract IERC20,uint256,contract TokenHolder,address,address,address)`
- **Visibility**: external
- **Source Range**: 4094:2611:12

## Implementation

```solidity
function buy(bytes calldata buyingCode, IERC20 tokenCollateral, uint256 borrowAmount, TokenHolder tokenHolder, address inchRouter, address integratorFeeAddress, address whitelistedDex) external payable nonReentrant() {
    uint256 totalAmount = borrowAmount + msg.value;
    weth.approve(whitelistedDex, totalAmount);
    uint256 buyerContribution = msg.value;
    (bool success, ) = payable(address(weth)).call{value: buyerContribution}("");
    require(success, "WETH failed");
    tokenHolder.privilegedLoan(weth, borrowAmount);
    (success, ) = inchRouter.call(buyingCode);
    require(success, "Buy token failed");
    uint256 balanceCollateral = tokenCollateral.balanceOf(address(this));
    uint256 totalFee = (balanceCollateral * openingFee) / 10000;
    if (integratorFeeAddress == address(0)) {
        require(tokenCollateral.transfer(FEE_ADDRESS, totalFee), "Fee transfer failed");
    } else {
        uint256 halfFee = totalFee / 2;
        require(tokenCollateral.transfer(FEE_ADDRESS, halfFee), "Fee transfer to FEE_ADDRESS failed");
        require(tokenCollateral.transfer(integratorFeeAddress, totalFee - halfFee), "Fee transfer to integratorFeeAddress failed");
    }
    uint256 loanId = tokenHolder.loanConfirmation(borrowAmount, balanceCollateral - totalFee, address(tokenCollateral), msg.sender, buyerContribution);
    require(tokenCollateral.transfer(address(tokenHolder), balanceCollateral - totalFee), "Collateral transfer failed");
    loanRecords[msg.sender].push(LoanRecord(loanId, address(tokenHolder)));
    emit Buy(msg.sender, address(tokenCollateral), address(tokenHolder), loanId, borrowAmount + buyerContribution, balanceCollateral - totalFee, buyerContribution);
}
```

## Related Implementations

### nonReentrant()

- **Kind**: modifier
- **Source**: 3425:103:3
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/utils/ReentrancyGuardUpgradeable.sol:ReentrancyGuardUpgradeable:nonReentrant()`

```solidity
///  @dev Prevents a contract from calling itself, directly or indirectly.
///  Calling a `nonReentrant` function from another `nonReentrant`
///  function is not supported. It is possible to prevent this from happening
///  by making the `nonReentrant` function external, and making it call a
///  `private` function that does the actual work.
modifier nonReentrant() {
    _nonReentrantBefore();
    _;
    _nonReentrantAfter();
}
```

### _nonReentrantBefore()

- **Kind**: internal
- **Source**: 4185:292:3
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/utils/ReentrancyGuardUpgradeable.sol:ReentrancyGuardUpgradeable:_nonReentrantBefore()`

```solidity
function _nonReentrantBefore() private {
    _nonReentrantBeforeView();
    _reentrancyGuardStorageSlot().getUint256Slot().value = ENTERED;
}
```

### _nonReentrantBeforeView()

- **Kind**: internal
- **Source**: 4022:157:3
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/utils/ReentrancyGuardUpgradeable.sol:ReentrancyGuardUpgradeable:_nonReentrantBeforeView()`

```solidity
function _nonReentrantBeforeView() private view {
    if (_reentrancyGuardEntered()) {
        revert ReentrancyGuardReentrantCall();
    }
}
```

### _reentrancyGuardEntered()

- **Kind**: internal
- **Source**: 4915:151:3
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/utils/ReentrancyGuardUpgradeable.sol:ReentrancyGuardUpgradeable:_reentrancyGuardEntered()`

```solidity
///  @dev Returns true if the reentrancy guard is currently set to "entered", which indicates there is a
///  `nonReentrant` function in the call stack.
function _reentrancyGuardEntered() internal view returns (bool) {
    return _reentrancyGuardStorageSlot().getUint256Slot().value == ENTERED;
}
```

### _reentrancyGuardStorageSlot()

- **Kind**: internal
- **Source**: 5072:127:3
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/utils/ReentrancyGuardUpgradeable.sol:ReentrancyGuardUpgradeable:_reentrancyGuardStorageSlot()`

```solidity
function _reentrancyGuardStorageSlot() virtual internal pure returns (bytes32) {
    return REENTRANCY_GUARD_STORAGE;
}
```

### getUint256Slot(bytes32)

- **Kind**: internal
- **Source**: 2679:163:10
- **Link**: `lib/openzeppelin-contracts-upgradeable/lib/openzeppelin-contracts/contracts/utils/StorageSlot.sol:StorageSlot:getUint256Slot(bytes32)`

```solidity
///  @dev Returns a `Uint256Slot` with member `value` located at `slot`.
function getUint256Slot(bytes32 slot) internal pure returns (Uint256Slot storage r) {
    assembly ("memory-safe") {
        r.slot := slot
    }
}
```

### _nonReentrantAfter()

- **Kind**: internal
- **Source**: 4483:253:3
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/utils/ReentrancyGuardUpgradeable.sol:ReentrancyGuardUpgradeable:_nonReentrantAfter()`

```solidity
function _nonReentrantAfter() private {
    _reentrancyGuardStorageSlot().getUint256Slot().value = NOT_ENTERED;
}
```

## External Calls

- **IERC20::approve(address,uint256)**
- **unknown::unknown**
- **TokenHolder::privilegedLoan(contract IERC20,uint256)**
- **address::call(bytes calldata)**
- **IERC20::balanceOf(address)**
- **IERC20::transfer(address,uint256)**
- **TokenHolder::loanConfirmation(uint256,uint256,address,address,uint256)**

## Native Transfers

- **inchRouter** (function parameter)
- **tokenCollateral** (function parameter)

## State Variable Reads

- **weth** (`contract IERC20`) [lib/openzeppelin-contracts-upgradeable/lib/openzeppelin-contracts/contracts/token/ERC20/IERC20.sol/interface_IERC20.md]
- **openingFee** (`uint256`)
- **FEE_ADDRESS** (`address`)
- **ENTERED** (`uint256`)
- **REENTRANCY_GUARD_STORAGE** (`bytes32`)
- **NOT_ENTERED** (`uint256`)

## State Variable Writes

- **loanRecords** (`mapping(address => struct LoanRecord[])`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: BorrowerOperationsV6.buy(bytes,contract IERC20,uint256,contract TokenHolder,address,address,address) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: external
  └─ [1] 🔒 MODIFIER: ReentrancyGuardUpgradeable.nonReentrant() (NodeID: 1)
      💬 Args: [no args]
    ├─ [2] ⚙️ FUNCTION: ReentrancyGuardUpgradeable._nonReentrantBefore() (NodeID: 2)
    │   💬 Args: [no args]
    │   👁️  Def: private
    │ ├─ [3] ⚙️ FUNCTION: ReentrancyGuardUpgradeable._nonReentrantBeforeView() (NodeID: 3)
    │ │   💬 Args: [no args]
    │ │   👁️  Def: private
    │ │ └─ [4] ⚙️ FUNCTION: ReentrancyGuardUpgradeable._reentrancyGuardEntered() (NodeID: 4)
    │ │     💬 Args: [no args]
    │ │     👁️  Def: internal
    │ │   ├─ [5] ⚙️ FUNCTION: ReentrancyGuardUpgradeable._reentrancyGuardStorageSlot() (NodeID: 5)
    │ │   │   💬 Args: [no args]
    │ │   │   👁️  Def: internal
    │ │   └─ [5] ⚙️ FUNCTION: StorageSlot.getUint256Slot(bytes32) (NodeID: 6)
    │ │       💬 Args: [_reentrancyGuardStorageSlot()]
    │ │       👁️  Def: internal
    │ ├─ [3] ⚙️ FUNCTION: ReentrancyGuardUpgradeable._reentrancyGuardStorageSlot() (NodeID: 7)
    │ │   💬 Args: [no args]
    │ │   👁️  Def: internal
    │ └─ [3] ⚙️ FUNCTION: StorageSlot.getUint256Slot(bytes32) (NodeID: 8)
    │     💬 Args: [_reentrancyGuardStorageSlot()]
    │     👁️  Def: internal
    └─ [2] ⚙️ FUNCTION: ReentrancyGuardUpgradeable._nonReentrantAfter() (NodeID: 9)
        💬 Args: [no args]
        👁️  Def: private
      ├─ [3] ⚙️ FUNCTION: ReentrancyGuardUpgradeable._reentrancyGuardStorageSlot() (NodeID: 10)
      │   💬 Args: [no args]
      │   👁️  Def: internal
      └─ [3] ⚙️ FUNCTION: StorageSlot.getUint256Slot(bytes32) (NodeID: 11)
          💬 Args: [_reentrancyGuardStorageSlot()]
          👁️  Def: internal
```
