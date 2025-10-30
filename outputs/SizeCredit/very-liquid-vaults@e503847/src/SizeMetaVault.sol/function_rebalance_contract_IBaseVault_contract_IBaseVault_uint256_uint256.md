# Function: rebalance(contract IBaseVault,contract IBaseVault,uint256,uint256)

**Contract**: [src/SizeMetaVault.sol/contract_SizeMetaVault.md]

## Metadata

- **Contract**: SizeMetaVault
- **Signature**: `rebalance(contract IBaseVault,contract IBaseVault,uint256,uint256)`
- **Visibility**: external
- **Source Range**: 12029:1227:59

## Implementation

```solidity
/// @notice Rebalances assets between two strategies
///  @dev Transfers assets from one strategy to another and skims the destination
///       Does not check that the strategyFrom is a whitelisted strategy to allow for rebalancing from removed strategies
function rebalance(IBaseVault strategyFrom, IBaseVault strategyTo, uint256 amount, uint256 minAmount) external nonReentrant() notPaused() onlyAuth(STRATEGIST_ROLE) {
    if (!isStrategy(strategyTo)) {
        revert InvalidStrategy(address(strategyTo));
    }
    if (amount == 0) {
        revert NullAmount();
    }
    uint256 totalAssetBefore = strategyTo.totalAssets();
    uint256 balanceBefore = IERC20(asset()).balanceOf(address(this));
    strategyFrom.withdraw(amount, address(this), address(this));
    uint256 balanceAfter = IERC20(asset()).balanceOf(address(this));
    uint256 assets = balanceAfter - balanceBefore;
    IERC20(asset()).forceApprove(address(strategyTo), assets);
    strategyTo.deposit(assets, address(this));
    uint256 transferredAmount = strategyTo.totalAssets() - totalAssetBefore;
    if (transferredAmount < minAmount) {
        revert TransferredAmountLessThanMin(transferredAmount, minAmount);
    }
    emit Rebalance(address(strategyFrom), address(strategyTo), assets);
}
```

## Related Implementations

### isStrategy(contract IBaseVault)

- **Kind**: internal
- **Source**: 19079:286:59
- **Link**: `src/SizeMetaVault.sol:SizeMetaVault:isStrategy(contract IBaseVault)`

```solidity
/// @notice Returns true if the strategy is in the vault
function isStrategy(IBaseVault strategy) public view returns (bool) {
    uint256 length = strategies.length;
    for (uint256 i = 0; i < length; i++) {
        if (strategies[i] == strategy) {
            return true;
        }
    }
    return false;
}
```

### asset()

- **Kind**: internal
- **Source**: 6882:153:17
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/token/ERC20/extensions/ERC4626Upgradeable.sol:ERC4626Upgradeable:asset()`

```solidity
/// @dev See {IERC4626-asset}. 
function asset() virtual public view returns (address) {
    ERC4626Storage storage $ = _getERC4626Storage();
    return address($._asset);
}
```

### _getERC4626Storage()

- **Kind**: internal
- **Source**: 4088:159:17
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/token/ERC20/extensions/ERC4626Upgradeable.sol:ERC4626Upgradeable:_getERC4626Storage()`

```solidity
function _getERC4626Storage() private pure returns (ERC4626Storage storage $) {
    assembly {
        $.slot := ERC4626StorageLocation
    }
}
```

### onlyAuth(bytes32)

- **Kind**: modifier
- **Source**: 4644:193:56
- **Link**: `src/BaseVault.sol:BaseVault:onlyAuth(bytes32)`

```solidity
/// @notice Modifier to restrict function access to addresses with specific roles
///  @dev Reverts if the caller doesn't have the required role
modifier onlyAuth(bytes32 role) {
    if (!auth.hasRole(role, msg.sender)) {
        revert IAccessControl.AccessControlUnauthorizedAccount(msg.sender, role);
    }
    _;
}
```

### notPaused()

- **Kind**: modifier
- **Source**: 4981:102:56
- **Link**: `src/BaseVault.sol:BaseVault:notPaused()`

```solidity
/// @notice Modifier to ensure the contract is not paused
///  @dev Checks both local pause state and global pause state from Auth
modifier notPaused() {
    if (paused() || auth.paused()) revert EnforcedPause();
    _;
}
```

### paused()

- **Kind**: internal
- **Source**: 2496:145:21
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/utils/PausableUpgradeable.sol:PausableUpgradeable:paused()`

```solidity
///  @dev Returns true if the contract is paused, and false otherwise.
function paused() virtual public view returns (bool) {
    PausableStorage storage $ = _getPausableStorage();
    return $._paused;
}
```

### _getPausableStorage()

- **Kind**: internal
- **Source**: 1147:162:21
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/utils/PausableUpgradeable.sol:PausableUpgradeable:_getPausableStorage()`

```solidity
function _getPausableStorage() private pure returns (PausableStorage storage $) {
    assembly {
        $.slot := PausableStorageLocation
    }
}
```

### nonReentrant()

- **Kind**: modifier
- **Source**: 3361:103:22
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
- **Source**: 3470:384:22
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/utils/ReentrancyGuardUpgradeable.sol:ReentrancyGuardUpgradeable:_nonReentrantBefore()`

```solidity
function _nonReentrantBefore() private {
    ReentrancyGuardStorage storage $ = _getReentrancyGuardStorage();
    if ($._status == ENTERED) {
        revert ReentrancyGuardReentrantCall();
    }
    $._status = ENTERED;
}
```

### _getReentrancyGuardStorage()

- **Kind**: internal
- **Source**: 2395:183:22
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/utils/ReentrancyGuardUpgradeable.sol:ReentrancyGuardUpgradeable:_getReentrancyGuardStorage()`

```solidity
function _getReentrancyGuardStorage() private pure returns (ReentrancyGuardStorage storage $) {
    assembly {
        $.slot := ReentrancyGuardStorageLocation
    }
}
```

### _nonReentrantAfter()

- **Kind**: internal
- **Source**: 3860:283:22
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/utils/ReentrancyGuardUpgradeable.sol:ReentrancyGuardUpgradeable:_nonReentrantAfter()`

```solidity
function _nonReentrantAfter() private {
    ReentrancyGuardStorage storage $ = _getReentrancyGuardStorage();
    $._status = NOT_ENTERED;
}
```

## External Calls

- **IBaseVault::totalAssets()**
- **IERC20::balanceOf(address)**
- **IBaseVault::withdraw(uint256,address,address)**
- **IERC20::forceApprove(contract IERC20,address,uint256)**
- **IBaseVault::deposit(uint256,address)**

## State Variable Reads

- **strategies** (`contract IBaseVault[]`) [src/IBaseVault.sol/interface_IBaseVault.md]
- **auth** (`contract Auth`) [src/utils/Auth.sol/contract_Auth.md]
- **ENTERED** (`uint256`)
- **NOT_ENTERED** (`uint256`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: SizeMetaVault.rebalance(contract IBaseVault,contract IBaseVault,uint256,uint256) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: external
  ├─ [1] ⚙️ FUNCTION: SizeMetaVault.isStrategy(contract IBaseVault) (NodeID: 1)
  │   💬 Args: [strategyTo]
  │   👁️  Def: public
  ├─ [1] ⚙️ FUNCTION: ERC4626Upgradeable.asset() (NodeID: 2)
  │   💬 Args: [no args]
  │   👁️  Def: public
  │ └─ [2] ⚙️ FUNCTION: ERC4626Upgradeable._getERC4626Storage() (NodeID: 3)
  │     💬 Args: [no args]
  │     👁️  Def: private
  ├─ [1] ⚙️ FUNCTION: ERC4626Upgradeable.asset() (NodeID: 4)
  │   💬 Args: [no args]
  │   👁️  Def: public
  │ └─ [2] ⚙️ FUNCTION: ERC4626Upgradeable._getERC4626Storage() (NodeID: 5)
  │     💬 Args: [no args]
  │     👁️  Def: private
  ├─ [1] ⚙️ FUNCTION: ERC4626Upgradeable.asset() (NodeID: 6)
  │   💬 Args: [no args]
  │   👁️  Def: public
  │ └─ [2] ⚙️ FUNCTION: ERC4626Upgradeable._getERC4626Storage() (NodeID: 7)
  │     💬 Args: [no args]
  │     👁️  Def: private
  ├─ [1] 🔒 MODIFIER: BaseVault.onlyAuth(bytes32) (NodeID: 8)
  │   💬 Args: [STRATEGIST_ROLE]
  ├─ [1] 🔒 MODIFIER: BaseVault.notPaused() (NodeID: 9)
  │   💬 Args: [no args]
  │ └─ [2] ⚙️ FUNCTION: PausableUpgradeable.paused() (NodeID: 10)
  │     💬 Args: [no args]
  │     👁️  Def: public
  │   └─ [3] ⚙️ FUNCTION: PausableUpgradeable._getPausableStorage() (NodeID: 11)
  │       💬 Args: [no args]
  │       👁️  Def: private
  └─ [1] 🔒 MODIFIER: ReentrancyGuardUpgradeable.nonReentrant() (NodeID: 12)
      💬 Args: [no args]
    ├─ [2] ⚙️ FUNCTION: ReentrancyGuardUpgradeable._nonReentrantBefore() (NodeID: 13)
    │   💬 Args: [no args]
    │   👁️  Def: private
    │ └─ [3] ⚙️ FUNCTION: ReentrancyGuardUpgradeable._getReentrancyGuardStorage() (NodeID: 14)
    │     💬 Args: [no args]
    │     👁️  Def: private
    └─ [2] ⚙️ FUNCTION: ReentrancyGuardUpgradeable._nonReentrantAfter() (NodeID: 15)
        💬 Args: [no args]
        👁️  Def: private
      └─ [3] ⚙️ FUNCTION: ReentrancyGuardUpgradeable._getReentrancyGuardStorage() (NodeID: 16)
          💬 Args: [no args]
          👁️  Def: private
```

## Documentation

### Function Documentation

@notice Rebalances assets between two strategies
 @dev Transfers assets from one strategy to another and skims the destination
      Does not check that the strategyFrom is a whitelisted strategy to allow for rebalancing from removed strategies
