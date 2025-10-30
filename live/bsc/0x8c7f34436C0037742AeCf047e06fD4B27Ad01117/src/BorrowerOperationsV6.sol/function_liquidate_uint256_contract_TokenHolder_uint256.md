# Function: liquidate(uint256,contract TokenHolder,uint256)

**Contract**: [src/BorrowerOperationsV6.sol/contract_BorrowerOperationsV6.md]

## Metadata

- **Contract**: BorrowerOperationsV6
- **Signature**: `liquidate(uint256,contract TokenHolder,uint256)`
- **Visibility**: external
- **Source Range**: 9336:1396:12

## Implementation

```solidity
function liquidate(uint256 loanId, TokenHolder tokenHolder, uint256 closingPositionSize) external onlyAdmin() nonReentrant() {
    (, uint256 amount, Collateral memory collateral, uint256 collateralAmount, uint256 timestamp, address borrower, ) = tokenHolder.loans(loanId);
    tokenHolder.privilegedLoan(IERC20(collateral.collateralAddress), collateralAmount);
    tokenHolder.repayLoan(loanId, true);
    IERC20(collateral.collateralAddress).transfer(msg.sender, collateralAmount);
    emit Liquidation(borrower, address(collateral.collateralAddress), address(tokenHolder), loanId, closingPositionSize, amount);
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

## External Calls

- **TokenHolder::loans(uint256)**
- **TokenHolder::privilegedLoan(contract IERC20,uint256)**
- **TokenHolder::repayLoan(uint256,bool)**
- **IERC20::transfer(address,uint256)**

## Native Transfers

- **unknown** (computed)

## State Variable Reads

- **ENTERED** (`uint256`)
- **REENTRANCY_GUARD_STORAGE** (`bytes32`)
- **NOT_ENTERED** (`uint256`)
- **admin** (`address`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: BorrowerOperationsV6.liquidate(uint256,contract TokenHolder,uint256) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: external
  ├─ [1] 🔒 MODIFIER: ReentrancyGuardUpgradeable.nonReentrant() (NodeID: 1)
  │   💬 Args: [no args]
  │ ├─ [2] ⚙️ FUNCTION: ReentrancyGuardUpgradeable._nonReentrantBefore() (NodeID: 2)
  │ │   💬 Args: [no args]
  │ │   👁️  Def: private
  │ │ ├─ [3] ⚙️ FUNCTION: ReentrancyGuardUpgradeable._nonReentrantBeforeView() (NodeID: 3)
  │ │ │   💬 Args: [no args]
  │ │ │   👁️  Def: private
  │ │ │ └─ [4] ⚙️ FUNCTION: ReentrancyGuardUpgradeable._reentrancyGuardEntered() (NodeID: 4)
  │ │ │     💬 Args: [no args]
  │ │ │     👁️  Def: internal
  │ │ │   ├─ [5] ⚙️ FUNCTION: ReentrancyGuardUpgradeable._reentrancyGuardStorageSlot() (NodeID: 5)
  │ │ │   │   💬 Args: [no args]
  │ │ │   │   👁️  Def: internal
  │ │ │   └─ [5] ⚙️ FUNCTION: StorageSlot.getUint256Slot(bytes32) (NodeID: 6)
  │ │ │       💬 Args: [_reentrancyGuardStorageSlot()]
  │ │ │       👁️  Def: internal
  │ │ ├─ [3] ⚙️ FUNCTION: ReentrancyGuardUpgradeable._reentrancyGuardStorageSlot() (NodeID: 7)
  │ │ │   💬 Args: [no args]
  │ │ │   👁️  Def: internal
  │ │ └─ [3] ⚙️ FUNCTION: StorageSlot.getUint256Slot(bytes32) (NodeID: 8)
  │ │     💬 Args: [_reentrancyGuardStorageSlot()]
  │ │     👁️  Def: internal
  │ └─ [2] ⚙️ FUNCTION: ReentrancyGuardUpgradeable._nonReentrantAfter() (NodeID: 9)
  │     💬 Args: [no args]
  │     👁️  Def: private
  │   ├─ [3] ⚙️ FUNCTION: ReentrancyGuardUpgradeable._reentrancyGuardStorageSlot() (NodeID: 10)
  │   │   💬 Args: [no args]
  │   │   👁️  Def: internal
  │   └─ [3] ⚙️ FUNCTION: StorageSlot.getUint256Slot(bytes32) (NodeID: 11)
  │       💬 Args: [_reentrancyGuardStorageSlot()]
  │       👁️  Def: internal
  └─ [1] 🔒 MODIFIER: BorrowerOperationsV6.onlyAdmin() (NodeID: 12)
      💬 Args: [no args]
```
