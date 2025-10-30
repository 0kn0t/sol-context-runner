# Interface: IERC4626Escrow

## Metadata

- **Name**: IERC4626Escrow
- **Type**: Interface
- **Path**: src/Dependencies/IERC4626Escrow.sol

## Implements Interfaces

- **IEscrow** [src/Dependencies/IEscrow.sol/interface_IEscrow.md]

## Events

### ProfitClaimed (inherited from IEscrow)

```solidity
event ProfitClaimed(uint256 profitAmount);
```

## Public/External Functions

### depositToExternalVault(uint256,uint256)

- **Signature**: `depositToExternalVault(uint256,uint256)`
- **Visibility**: external
- **Source Range**: 149:85:31

**Signature:**
```solidity
function depositToExternalVault(uint256 assetsToDeposit, uint256 minShares) external;;
```

### redeemFromExternalVault(uint256,uint256)

- **Signature**: `redeemFromExternalVault(uint256,uint256)`
- **Visibility**: external
- **Source Range**: 239:85:31

**Signature:**
```solidity
function redeemFromExternalVault(uint256 sharesToRedeem, uint256 minAssets) external;;
```

### totalAssetsDeposited() (inherited from IEscrow)

- **Signature**: `totalAssetsDeposited()`
- **Visibility**: external
- **Source Range**: 139:64:34

**Signature:**
```solidity
function totalAssetsDeposited() external view returns (uint256);;
```

### totalBalance() (inherited from IEscrow)

- **Signature**: `totalBalance()`
- **Visibility**: external
- **Source Range**: 208:56:34

**Signature:**
```solidity
function totalBalance() external view returns (uint256);;
```

### onDeposit(uint256) (inherited from IEscrow)

- **Signature**: `onDeposit(uint256)`
- **Visibility**: external
- **Source Range**: 269:49:34

**Signature:**
```solidity
function onDeposit(uint256 assetAmount) external;;
```

### onWithdraw(uint256) (inherited from IEscrow)

- **Signature**: `onWithdraw(uint256)`
- **Visibility**: external
- **Source Range**: 323:68:34

**Signature:**
```solidity
function onWithdraw(uint256 assetAmount) external returns (uint256);;
```

### previewWithdraw(uint256) (inherited from IEscrow)

- **Signature**: `previewWithdraw(uint256)`
- **Visibility**: external
- **Source Range**: 396:78:34

**Signature:**
```solidity
function previewWithdraw(uint256 assetAmount) external view returns (uint256);;
```

### claimProfit() (inherited from IEscrow)

- **Signature**: `claimProfit()`
- **Visibility**: external
- **Source Range**: 479:32:34

**Signature:**
```solidity
function claimProfit() external;;
```

### onMigrateSource(address) (inherited from IEscrow)

- **Signature**: `onMigrateSource(address)`
- **Visibility**: external
- **Source Range**: 516:53:34

**Signature:**
```solidity
function onMigrateSource(address newEscrow) external;;
```

### onMigrateTarget(uint256) (inherited from IEscrow)

- **Signature**: `onMigrateTarget(uint256)`
- **Visibility**: external
- **Source Range**: 574:50:34

**Signature:**
```solidity
function onMigrateTarget(uint256 amount) external;;
```
