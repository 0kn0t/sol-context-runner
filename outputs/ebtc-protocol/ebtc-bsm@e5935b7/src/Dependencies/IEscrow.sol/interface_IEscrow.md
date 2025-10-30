# Interface: IEscrow

## Metadata

- **Name**: IEscrow
- **Type**: Interface
- **Path**: src/Dependencies/IEscrow.sol

## Events

### ProfitClaimed

```solidity
event ProfitClaimed(uint256 profitAmount);
```

## Public/External Functions

### totalAssetsDeposited()

- **Signature**: `totalAssetsDeposited()`
- **Visibility**: external
- **Source Range**: 139:64:34

**Signature:**
```solidity
function totalAssetsDeposited() external view returns (uint256);;
```

### totalBalance()

- **Signature**: `totalBalance()`
- **Visibility**: external
- **Source Range**: 208:56:34

**Signature:**
```solidity
function totalBalance() external view returns (uint256);;
```

### onDeposit(uint256)

- **Signature**: `onDeposit(uint256)`
- **Visibility**: external
- **Source Range**: 269:49:34

**Signature:**
```solidity
function onDeposit(uint256 assetAmount) external;;
```

### onWithdraw(uint256)

- **Signature**: `onWithdraw(uint256)`
- **Visibility**: external
- **Source Range**: 323:68:34

**Signature:**
```solidity
function onWithdraw(uint256 assetAmount) external returns (uint256);;
```

### previewWithdraw(uint256)

- **Signature**: `previewWithdraw(uint256)`
- **Visibility**: external
- **Source Range**: 396:78:34

**Signature:**
```solidity
function previewWithdraw(uint256 assetAmount) external view returns (uint256);;
```

### claimProfit()

- **Signature**: `claimProfit()`
- **Visibility**: external
- **Source Range**: 479:32:34

**Signature:**
```solidity
function claimProfit() external;;
```

### onMigrateSource(address)

- **Signature**: `onMigrateSource(address)`
- **Visibility**: external
- **Source Range**: 516:53:34

**Signature:**
```solidity
function onMigrateSource(address newEscrow) external;;
```

### onMigrateTarget(uint256)

- **Signature**: `onMigrateTarget(uint256)`
- **Visibility**: external
- **Source Range**: 574:50:34

**Signature:**
```solidity
function onMigrateTarget(uint256 amount) external;;
```
