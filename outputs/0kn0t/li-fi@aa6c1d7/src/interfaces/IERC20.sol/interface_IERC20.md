# Interface: IERC20

## Metadata

- **Name**: IERC20
- **Type**: Interface
- **Path**: src/interfaces/IERC20.sol

## Public/External Functions

### balanceOf(address)

- **Signature**: `balanceOf(address)`
- **Visibility**: external
- **Source Range**: 91:68:38

**Signature:**
```solidity
function balanceOf(address account) external view returns (uint256);;
```

### allowance(address,address)

- **Signature**: `allowance(address,address)`
- **Visibility**: external
- **Source Range**: 164:83:38

**Signature:**
```solidity
function allowance(address owner, address spender) external view returns (uint256);;
```

### approve(address,uint256)

- **Signature**: `approve(address,uint256)`
- **Visibility**: external
- **Source Range**: 252:74:38

**Signature:**
```solidity
function approve(address spender, uint256 amount) external returns (bool);;
```

### transferFrom(address,address,uint256)

- **Signature**: `transferFrom(address,address,uint256)`
- **Visibility**: external
- **Source Range**: 331:97:38

**Signature:**
```solidity
function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);;
```
