# Interface: IERC20Detailed

## Metadata

- **Name**: IERC20Detailed
- **Type**: Interface
- **Path**: lib/aave-v3-origin/src/contracts/dependencies/openzeppelin/contracts/IERC20Detailed.sol

## Implements Interfaces

- **IERC20** [lib/aave-v3-origin/src/contracts/dependencies/openzeppelin/contracts/IERC20.sol/interface_IERC20.md]

## Events

### Transfer (inherited from IERC20)

```solidity
///  @dev Emitted when `value` tokens are moved from one account (`from`) to
///  another (`to`).
///  Note that `value` may be zero.
event Transfer(address indexed from, address indexed to, uint256 value);
```

### Approval (inherited from IERC20)

```solidity
///  @dev Emitted when the allowance of a `spender` for an `owner` is set by
///  a call to {approve}. `value` is the new allowance.
event Approval(address indexed owner, address indexed spender, uint256 value);
```

## Public/External Functions

### name()

- **Signature**: `name()`
- **Visibility**: external
- **Source Range**: 135:54:4

**Signature:**
```solidity
function name() external view returns (string memory);;
```

### symbol()

- **Signature**: `symbol()`
- **Visibility**: external
- **Source Range**: 193:56:4

**Signature:**
```solidity
function symbol() external view returns (string memory);;
```

### decimals()

- **Signature**: `decimals()`
- **Visibility**: external
- **Source Range**: 253:50:4

**Signature:**
```solidity
function decimals() external view returns (uint8);;
```

### totalSupply() (inherited from IERC20)

- **Signature**: `totalSupply()`
- **Visibility**: external
- **Source Range**: 214:55:3

**Signature:**
```solidity
///  @dev Returns the amount of tokens in existence.
function totalSupply() external view returns (uint256);;
```

### balanceOf(address) (inherited from IERC20)

- **Signature**: `balanceOf(address)`
- **Visibility**: external
- **Source Range**: 344:68:3

**Signature:**
```solidity
///  @dev Returns the amount of tokens owned by `account`.
function balanceOf(address account) external view returns (uint256);;
```

### transfer(address,uint256) (inherited from IERC20)

- **Signature**: `transfer(address,uint256)`
- **Visibility**: external
- **Source Range**: 616:77:3

**Signature:**
```solidity
///  @dev Moves `amount` tokens from the caller's account to `recipient`.
///  Returns a boolean value indicating whether the operation succeeded.
///  Emits a {Transfer} event.
function transfer(address recipient, uint256 amount) external returns (bool);;
```

### allowance(address,address) (inherited from IERC20)

- **Signature**: `allowance(address,address)`
- **Visibility**: external
- **Source Range**: 952:83:3

**Signature:**
```solidity
///  @dev Returns the remaining number of tokens that `spender` will be
///  allowed to spend on behalf of `owner` through {transferFrom}. This is
///  zero by default.
///  This value changes when {approve} or {transferFrom} are called.
function allowance(address owner, address spender) external view returns (uint256);;
```

### approve(address,uint256) (inherited from IERC20)

- **Signature**: `approve(address,uint256)`
- **Visibility**: external
- **Source Range**: 1658:74:3

**Signature:**
```solidity
///  @dev Sets `amount` as the allowance of `spender` over the caller's tokens.
///  Returns a boolean value indicating whether the operation succeeded.
///  IMPORTANT: Beware that changing an allowance with this method brings the risk
///  that someone may use both the old and the new allowance by unfortunate
///  transaction ordering. One possible solution to mitigate this race
///  condition is to first reduce the spender's allowance to 0 and set the
///  desired value afterwards:
///  https://github.com/ethereum/EIPs/issues/20#issuecomment-263524729
///  Emits an {Approval} event.
function approve(address spender, uint256 amount) external returns (bool);;
```

### transferFrom(address,address,uint256) (inherited from IERC20)

- **Signature**: `transferFrom(address,address,uint256)`
- **Visibility**: external
- **Source Range**: 2019:97:3

**Signature:**
```solidity
///  @dev Moves `amount` tokens from `sender` to `recipient` using the
///  allowance mechanism. `amount` is then deducted from the caller's
///  allowance.
///  Returns a boolean value indicating whether the operation succeeded.
///  Emits a {Transfer} event.
function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);;
```
