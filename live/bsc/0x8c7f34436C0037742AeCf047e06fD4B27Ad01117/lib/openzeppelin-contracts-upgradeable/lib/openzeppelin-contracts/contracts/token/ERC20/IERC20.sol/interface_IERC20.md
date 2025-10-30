# Interface: IERC20

## Metadata

- **Name**: IERC20
- **Type**: Interface
- **Path**: lib/openzeppelin-contracts-upgradeable/lib/openzeppelin-contracts/contracts/token/ERC20/IERC20.sol
- **Documentation**:  @dev Interface of the ERC-20 standard as defined in the ERC.

## Events

### Transfer

```solidity
///  @dev Emitted when `value` tokens are moved from one account (`from`) to
///  another (`to`).
///  Note that `value` may be zero.
event Transfer(address indexed from, address indexed to, uint256 value);
```

### Approval

```solidity
///  @dev Emitted when the allowance of a `spender` for an `owner` is set by
///  a call to {approve}. `value` is the new allowance.
event Approval(address indexed owner, address indexed spender, uint256 value);
```

## Public/External Functions

### totalSupply()

- **Signature**: `totalSupply()`
- **Visibility**: external
- **Source Range**: 776:55:8

**Signature:**
```solidity
///  @dev Returns the value of tokens in existence.
function totalSupply() external view returns (uint256);;
```

### balanceOf(address)

- **Signature**: `balanceOf(address)`
- **Visibility**: external
- **Source Range**: 913:68:8

**Signature:**
```solidity
///  @dev Returns the value of tokens owned by `account`.
function balanceOf(address account) external view returns (uint256);;
```

### transfer(address,uint256)

- **Signature**: `transfer(address,uint256)`
- **Visibility**: external
- **Source Range**: 1205:69:8

**Signature:**
```solidity
///  @dev Moves a `value` amount of tokens from the caller's account to `to`.
///  Returns a boolean value indicating whether the operation succeeded.
///  Emits a {Transfer} event.
function transfer(address to, uint256 value) external returns (bool);;
```

### allowance(address,address)

- **Signature**: `allowance(address,address)`
- **Visibility**: external
- **Source Range**: 1549:83:8

**Signature:**
```solidity
///  @dev Returns the remaining number of tokens that `spender` will be
///  allowed to spend on behalf of `owner` through {transferFrom}. This is
///  zero by default.
///  This value changes when {approve} or {transferFrom} are called.
function allowance(address owner, address spender) external view returns (uint256);;
```

### approve(address,uint256)

- **Signature**: `approve(address,uint256)`
- **Visibility**: external
- **Source Range**: 2310:73:8

**Signature:**
```solidity
///  @dev Sets a `value` amount of tokens as the allowance of `spender` over the
///  caller's tokens.
///  Returns a boolean value indicating whether the operation succeeded.
///  IMPORTANT: Beware that changing an allowance with this method brings the risk
///  that someone may use both the old and the new allowance by unfortunate
///  transaction ordering. One possible solution to mitigate this race
///  condition is to first reduce the spender's allowance to 0 and set the
///  desired value afterwards:
///  https://github.com/ethereum/EIPs/issues/20#issuecomment-263524729
///  Emits an {Approval} event.
function approve(address spender, uint256 value) external returns (bool);;
```

### transferFrom(address,address,uint256)

- **Signature**: `transferFrom(address,address,uint256)`
- **Visibility**: external
- **Source Range**: 2691:87:8

**Signature:**
```solidity
///  @dev Moves a `value` amount of tokens from `from` to `to` using the
///  allowance mechanism. `value` is then deducted from the caller's
///  allowance.
///  Returns a boolean value indicating whether the operation succeeded.
///  Emits a {Transfer} event.
function transferFrom(address from, address to, uint256 value) external returns (bool);;
```
