# Function: multicall(bytes[])

**Contract**: [src/SizeMetaVault.sol/contract_SizeMetaVault.md]

## Metadata

- **Contract**: SizeMetaVault
- **Signature**: `multicall(bytes[])`
- **Visibility**: external
- **Source Range**: 1518:484:19
- **Inherited From**: MulticallUpgradeable

## Implementation

```solidity
///  @dev Receives and executes a batch of function calls on this contract.
///  @custom:oz-upgrades-unsafe-allow-reachable delegatecall
function multicall(bytes[] calldata data) virtual external returns (bytes[] memory results) {
    bytes memory context = (msg.sender == _msgSender()) ? new bytes(0) : msg.data[msg.data.length - _contextSuffixLength():];
    results = new bytes[](data.length);
    for (uint256 i = 0; i < data.length; i++) {
        results[i] = Address.functionDelegateCall(address(this), bytes.concat(data[i], context));
    }
    return results;
}
```

## Related Implementations

### _msgSender()

- **Kind**: internal
- **Source**: 887:96:18
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/utils/ContextUpgradeable.sol:ContextUpgradeable:_msgSender()`

```solidity
function _msgSender() virtual internal view returns (address) {
    return msg.sender;
}
```

### _contextSuffixLength()

- **Kind**: internal
- **Source**: 1094:97:18
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/utils/ContextUpgradeable.sol:ContextUpgradeable:_contextSuffixLength()`

```solidity
function _contextSuffixLength() virtual internal view returns (uint256) {
    return 0;
}
```

### functionDelegateCall(address,bytes)

- **Kind**: internal
- **Source**: 3916:253:41
- **Link**: `lib/openzeppelin-contracts/contracts/utils/Address.sol:Address:functionDelegateCall(address,bytes)`

```solidity
///  @dev Same as {xref-Address-functionCall-address-bytes-}[`functionCall`],
///  but performing a delegate call.
function functionDelegateCall(address target, bytes memory data) internal returns (bytes memory) {
    (bool success, bytes memory returndata) = target.delegatecall(data);
    return verifyCallResultFromTarget(target, success, returndata);
}
```

### verifyCallResultFromTarget(address,bool,bytes)

- **Kind**: internal
- **Source**: 4437:582:41
- **Link**: `lib/openzeppelin-contracts/contracts/utils/Address.sol:Address:verifyCallResultFromTarget(address,bool,bytes)`

```solidity
///  @dev Tool to verify that a low level call to smart-contract was successful, and reverts if the target
///  was not a contract or bubbling up the revert reason (falling back to {Errors.FailedCall}) in case
///  of an unsuccessful call.
function verifyCallResultFromTarget(address target, bool success, bytes memory returndata) internal view returns (bytes memory) {
    if (!success) {
        _revert(returndata);
    } else {
        if ((returndata.length == 0) && (target.code.length == 0)) {
            revert AddressEmptyCode(target);
        }
        return returndata;
    }
}
```

### _revert(bytes)

- **Kind**: internal
- **Source**: 5559:487:41
- **Link**: `lib/openzeppelin-contracts/contracts/utils/Address.sol:Address:_revert(bytes)`

```solidity
///  @dev Reverts with returndata if present. Otherwise reverts with {Errors.FailedCall}.
function _revert(bytes memory returndata) private pure {
    if (returndata.length > 0) {
        assembly ("memory-safe") {
            let returndata_size := mload(returndata)
            revert(add(32, returndata), returndata_size)
        }
    } else {
        revert Errors.FailedCall();
    }
}
```

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: MulticallUpgradeable.multicall(bytes[]) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: external
  ├─ [1] ⚙️ FUNCTION: ContextUpgradeable._msgSender() (NodeID: 1)
  │   💬 Args: [no args]
  │   👁️  Def: internal
  ├─ [1] ⚙️ FUNCTION: ContextUpgradeable._contextSuffixLength() (NodeID: 2)
  │   💬 Args: [no args]
  │   👁️  Def: internal
  └─ [1] ⚙️ FUNCTION: Address.functionDelegateCall(address,bytes) (NodeID: 3)
      💬 Args: [address(this), bytes.concat(data[i], context)]
      👁️  Def: internal
    └─ [2] ⚙️ FUNCTION: Address.verifyCallResultFromTarget(address,bool,bytes) (NodeID: 4)
        💬 Args: [target, success, returndata]
        👁️  Def: internal
      └─ [3] ⚙️ FUNCTION: Address._revert(bytes) (NodeID: 5)
          💬 Args: [returndata]
          👁️  Def: private
```

## Documentation

### Function Documentation

 @dev Receives and executes a batch of function calls on this contract.
 @custom:oz-upgrades-unsafe-allow-reachable delegatecall
