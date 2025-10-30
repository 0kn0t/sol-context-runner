# Interface: ISafeSmartAccount

## Metadata

- **Name**: ISafeSmartAccount
- **Type**: Interface
- **Path**: lib/safe-utils/src/ISafeSmartAccount.sol

## Public/External Functions

### nonce()

- **Signature**: `nonce()`
- **Visibility**: external
- **Source Range**: 167:49:120

**Signature:**
```solidity
function nonce() external view returns (uint256);;
```

### getTransactionHash(address,uint256,bytes,enum Enum.Operation,uint256,uint256,uint256,address,address,uint256)

- **Signature**: `getTransactionHash(address,uint256,bytes,enum Enum.Operation,uint256,uint256,uint256,address,address,uint256)`
- **Visibility**: external
- **Source Range**: 222:332:120

**Signature:**
```solidity
function getTransactionHash(address to, uint256 value, bytes calldata data, Enum.Operation operation, uint256 safeTxGas, uint256 baseGas, uint256 gasPrice, address gasToken, address refundReceiver, uint256 _nonce) external view returns (bytes32);;
```

### execTransaction(address,uint256,bytes,enum Enum.Operation,uint256,uint256,uint256,address,address payable,bytes)

- **Signature**: `execTransaction(address,uint256,bytes,enum Enum.Operation,uint256,uint256,uint256,address,address payable,bytes)`
- **Visibility**: external
- **Source Range**: 560:354:120

**Signature:**
```solidity
function execTransaction(address to, uint256 value, bytes calldata data, Enum.Operation operation, uint256 safeTxGas, uint256 baseGas, uint256 gasPrice, address gasToken, address payable refundReceiver, bytes memory signatures) external payable returns (bool success);;
```
