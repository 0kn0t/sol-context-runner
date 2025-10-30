# Function: sell(uint256,bytes,contract TokenHolder,address,address,address)

**Contract**: [src/BorrowerOperationsV6.sol/contract_BorrowerOperationsV6.md]

## Metadata

- **Contract**: BorrowerOperationsV6
- **Signature**: `sell(uint256,bytes,contract TokenHolder,address,address,address)`
- **Visibility**: external
- **Source Range**: 6711:2619:12

## Implementation

```solidity
function sell(uint256 loanId, bytes calldata sellingCode, TokenHolder tokenHolder, address inchRouter, address integratorFeeAddress, address whitelistedDex) external payable nonReentrant() {
    (, uint256 borrowAmount, Collateral memory collateral, uint256 collateralAmount, , address borrower, uint256 userPaid) = tokenHolder.loans(loanId);
    tokenHolder.privilegedLoan(IERC20(collateral.collateralAddress), collateralAmount);
    IERC20(collateral.collateralAddress).approve(whitelistedDex, collateralAmount);
    (bool success, ) = inchRouter.call(sellingCode);
    require(success, "Sell token failed");
    uint256 closingPositionSize = weth.balanceOf(address(this));
    (bool success2, ) = payable(address(weth)).call{value: msg.value}("");
    require(success2, "WETH failed");
    weth.approve(address(tokenHolder), MAX_INT);
    tokenHolder.repayLoan(loanId, false);
    uint256 balance = weth.balanceOf(address(this));
    uint256 profit = (balance > userPaid) ? (balance - userPaid) : 0;
    uint256 totalFee = ((profit * profitFee) / 10000) + (((borrowAmount + userPaid) * openingFee) / 10000);
    if (integratorFeeAddress == address(0)) {
        require(weth.transfer(FEE_ADDRESS, totalFee), "Fee transfer failed");
    } else {
        require(weth.transfer(FEE_ADDRESS, totalFee / 2), "Fee transfer to FEE_ADDRESS failed");
        require(weth.transfer(integratorFeeAddress, totalFee - (totalFee / 2)), "Fee transfer to integratorFeeAddress failed");
    }
    weth.transfer(borrower, weth.balanceOf(address(this)));
    emit Sell(borrower, address(collateral.collateralAddress), address(tokenHolder), loanId, closingPositionSize, profit);
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

- **TokenHolder::loans(uint256)**
- **TokenHolder::privilegedLoan(contract IERC20,uint256)**
- **IERC20::approve(address,uint256)**
- **address::call(bytes calldata)**
- **IERC20::balanceOf(address)**
- **unknown::unknown**
- **TokenHolder::repayLoan(uint256,bool)**
- **IERC20::transfer(address,uint256)**

## Native Transfers

- **inchRouter** (function parameter)
- **weth** (state variable) [lib/openzeppelin-contracts-upgradeable/lib/openzeppelin-contracts/contracts/token/ERC20/IERC20.sol/interface_IERC20.md]

## State Variable Reads

- **weth** (`contract IERC20`) [lib/openzeppelin-contracts-upgradeable/lib/openzeppelin-contracts/contracts/token/ERC20/IERC20.sol/interface_IERC20.md]
- **MAX_INT** (`uint256`)
- **profitFee** (`uint256`)
- **openingFee** (`uint256`)
- **FEE_ADDRESS** (`address`)
- **ENTERED** (`uint256`)
- **REENTRANCY_GUARD_STORAGE** (`bytes32`)
- **NOT_ENTERED** (`uint256`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: BorrowerOperationsV6.sell(uint256,bytes,contract TokenHolder,address,address,address) (NodeID: 0)
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
