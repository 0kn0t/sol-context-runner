# Interface: IEbtcBSM

## Metadata

- **Name**: IEbtcBSM
- **Type**: Interface
- **Path**: src/Dependencies/IEbtcBSM.sol

## Events

### EscrowUpdated

```solidity
event EscrowUpdated(address indexed oldVault, address indexed newVault);
```

### FeeToSellUpdated

```solidity
event FeeToSellUpdated(uint256 oldFee, uint256 newFee);
```

### FeeToBuyUpdated

```solidity
event FeeToBuyUpdated(uint256 oldFee, uint256 newFee);
```

### AssetSold

```solidity
event AssetSold(uint256 assetAmountIn, uint256 ebtcAmountOut, uint256 feeAmount);
```

### AssetBought

```solidity
event AssetBought(uint256 ebtcAmountIn, uint256 assetAmountOut, uint256 feeAmount);
```

## Public/External Functions

### previewSellAsset(uint256)

- **Signature**: `previewSellAsset(uint256)`
- **Visibility**: external
- **Source Range**: 461:97:32

**Signature:**
```solidity
function previewSellAsset(uint256 _assetAmountIn) external view returns (uint256 _ebtcAmountOut);;
```

### sellAsset(uint256,address,uint256)

- **Signature**: `sellAsset(uint256,address,uint256)`
- **Visibility**: external
- **Source Range**: 563:128:32

**Signature:**
```solidity
function sellAsset(uint256 _assetAmountIn, address _recipient, uint256 _minOutAmount) external returns (uint256 _ebtcAmountOut);;
```

### sellAssetNoFee(uint256,address,uint256)

- **Signature**: `sellAssetNoFee(uint256,address,uint256)`
- **Visibility**: external
- **Source Range**: 696:133:32

**Signature:**
```solidity
function sellAssetNoFee(uint256 _assetAmountIn, address _recipient, uint256 _minOutAmount) external returns (uint256 _ebtcAmountOut);;
```

### previewBuyAsset(uint256)

- **Signature**: `previewBuyAsset(uint256)`
- **Visibility**: external
- **Source Range**: 834:96:32

**Signature:**
```solidity
function previewBuyAsset(uint256 _ebtcAmountIn) external view returns (uint256 _assetAmountOut);;
```

### buyAsset(uint256,address,uint256)

- **Signature**: `buyAsset(uint256,address,uint256)`
- **Visibility**: external
- **Source Range**: 935:127:32

**Signature:**
```solidity
function buyAsset(uint256 _ebtcAmountIn, address _recipient, uint256 _minOutAmount) external returns (uint256 _assetAmountOut);;
```

### buyAssetNoFee(uint256,address,uint256)

- **Signature**: `buyAssetNoFee(uint256,address,uint256)`
- **Visibility**: external
- **Source Range**: 1067:132:32

**Signature:**
```solidity
function buyAssetNoFee(uint256 _ebtcAmountIn, address _recipient, uint256 _minOutAmount) external returns (uint256 _assetAmountOut);;
```

### totalMinted()

- **Signature**: `totalMinted()`
- **Visibility**: external
- **Source Range**: 1204:55:32

**Signature:**
```solidity
function totalMinted() external view returns (uint256);;
```
