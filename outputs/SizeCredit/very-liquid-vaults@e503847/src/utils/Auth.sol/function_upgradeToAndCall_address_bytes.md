# Function: upgradeToAndCall(address,bytes)

**Contract**: [src/utils/Auth.sol/contract_Auth.md]

## Metadata

- **Contract**: Auth
- **Signature**: `upgradeToAndCall(address,bytes)`
- **Visibility**: public
- **Source Range**: 4161:214:14
- **Inherited From**: UUPSUpgradeable

## Implementation

```solidity
///  @dev Upgrade the implementation of the proxy to `newImplementation`, and subsequently execute the function call
///  encoded in `data`.
///  Calls {_authorizeUpgrade}.
///  Emits an {Upgraded} event.
///  @custom:oz-upgrades-unsafe-allow-reachable delegatecall
function upgradeToAndCall(address newImplementation, bytes memory data) virtual public payable onlyProxy() {
    _authorizeUpgrade(newImplementation);
    _upgradeToAndCallUUPS(newImplementation, data);
}
```

## Related Implementations

### _authorizeUpgrade(address)

- **Kind**: internal
- **Source**: 1916:103:63
- **Link**: `src/utils/Auth.sol:Auth:_authorizeUpgrade(address)`

```solidity
/// @notice Authorizes contract upgrades
///  @dev Only addresses with DEFAULT_ADMIN_ROLE can authorize upgrades
function _authorizeUpgrade(address newImplementation) override internal onlyRole(DEFAULT_ADMIN_ROLE) {}
```

### onlyRole(bytes32)

- **Kind**: modifier
- **Source**: 3149:76:11
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/access/AccessControlUpgradeable.sol:AccessControlUpgradeable:onlyRole(bytes32)`

```solidity
///  @dev Modifier that checks that an account has a specific role. Reverts
///  with an {AccessControlUnauthorizedAccount} error including the required role.
modifier onlyRole(bytes32 role) {
    _checkRole(role);
    _;
}
```

### _checkRole(bytes32)

- **Kind**: internal
- **Source**: 4148:103:11
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/access/AccessControlUpgradeable.sol:AccessControlUpgradeable:_checkRole(bytes32)`

```solidity
///  @dev Reverts with an {AccessControlUnauthorizedAccount} error if `_msgSender()`
///  is missing `role`. Overriding this function changes the behavior of the {onlyRole} modifier.
function _checkRole(bytes32 role) virtual internal view {
    _checkRole(role, _msgSender());
}
```

### _checkRole(bytes32,address)

- **Kind**: internal
- **Source**: 4381:197:11
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/access/AccessControlUpgradeable.sol:AccessControlUpgradeable:_checkRole(bytes32,address)`

```solidity
///  @dev Reverts with an {AccessControlUnauthorizedAccount} error if `account`
///  is missing `role`.
function _checkRole(bytes32 role, address account) virtual internal view {
    if (!hasRole(role, account)) {
        revert AccessControlUnauthorizedAccount(account, role);
    }
}
```

### _msgSender()

- **Kind**: internal
- **Source**: 887:96:18
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/utils/ContextUpgradeable.sol:ContextUpgradeable:_msgSender()`

```solidity
function _msgSender() virtual internal view returns (address) {
    return msg.sender;
}
```

### hasRole(bytes32,address)

- **Kind**: internal
- **Source**: 3732:207:11
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/access/AccessControlUpgradeable.sol:AccessControlUpgradeable:hasRole(bytes32,address)`

```solidity
///  @dev Returns `true` if `account` has been granted `role`.
function hasRole(bytes32 role, address account) virtual public view returns (bool) {
    AccessControlStorage storage $ = _getAccessControlStorage();
    return $._roles[role].hasRole[account];
}
```

### _getAccessControlStorage()

- **Kind**: internal
- **Source**: 2787:177:11
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/access/AccessControlUpgradeable.sol:AccessControlUpgradeable:_getAccessControlStorage()`

```solidity
function _getAccessControlStorage() private pure returns (AccessControlStorage storage $) {
    assembly {
        $.slot := AccessControlStorageLocation
    }
}
```

### _upgradeToAndCallUUPS(address,bytes)

- **Kind**: internal
- **Source**: 6032:538:14
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/proxy/utils/UUPSUpgradeable.sol:UUPSUpgradeable:_upgradeToAndCallUUPS(address,bytes)`

```solidity
///  @dev Performs an implementation upgrade with a security check for UUPS proxies, and additional setup call.
///  As a security check, {proxiableUUID} is invoked in the new implementation, and the return value
///  is expected to be the implementation slot in ERC-1967.
///  Emits an {IERC1967-Upgraded} event.
function _upgradeToAndCallUUPS(address newImplementation, bytes memory data) private {
    try IERC1822Proxiable(newImplementation).proxiableUUID() returns (bytes32 slot) {
        if (slot != ERC1967Utils.IMPLEMENTATION_SLOT) {
            revert UUPSUnsupportedProxiableUUID(slot);
        }
        ERC1967Utils.upgradeToAndCall(newImplementation, data);
    } catch {
        revert ERC1967Utils.ERC1967InvalidImplementation(newImplementation);
    }
}
```

### upgradeToAndCall(address,bytes)

- **Kind**: internal
- **Source**: 2264:344:35
- **Link**: `lib/openzeppelin-contracts/contracts/proxy/ERC1967/ERC1967Utils.sol:ERC1967Utils:upgradeToAndCall(address,bytes)`

```solidity
///  @dev Performs implementation upgrade with additional setup call if data is nonempty.
///  This function is payable only if the setup call is performed, otherwise `msg.value` is rejected
///  to avoid stuck value in the contract.
///  Emits an {IERC1967-Upgraded} event.
function upgradeToAndCall(address newImplementation, bytes memory data) internal {
    _setImplementation(newImplementation);
    emit IERC1967.Upgraded(newImplementation);
    if (data.length > 0) {
        Address.functionDelegateCall(newImplementation, data);
    } else {
        _checkNonPayable();
    }
}
```

### _setImplementation(address)

- **Kind**: internal
- **Source**: 1671:281:35
- **Link**: `lib/openzeppelin-contracts/contracts/proxy/ERC1967/ERC1967Utils.sol:ERC1967Utils:_setImplementation(address)`

```solidity
///  @dev Stores a new address in the ERC-1967 implementation slot.
function _setImplementation(address newImplementation) private {
    if (newImplementation.code.length == 0) {
        revert ERC1967InvalidImplementation(newImplementation);
    }
    StorageSlot.getAddressSlot(IMPLEMENTATION_SLOT).value = newImplementation;
}
```

### getAddressSlot(bytes32)

- **Kind**: internal
- **Source**: 1899:163:47
- **Link**: `lib/openzeppelin-contracts/contracts/utils/StorageSlot.sol:StorageSlot:getAddressSlot(bytes32)`

```solidity
///  @dev Returns an `AddressSlot` with member `value` located at `slot`.
function getAddressSlot(bytes32 slot) internal pure returns (AddressSlot storage r) {
    assembly ("memory-safe") {
        r.slot := slot
    }
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

### _checkNonPayable()

- **Kind**: internal
- **Source**: 6113:122:35
- **Link**: `lib/openzeppelin-contracts/contracts/proxy/ERC1967/ERC1967Utils.sol:ERC1967Utils:_checkNonPayable()`

```solidity
///  @dev Reverts if `msg.value` is not zero. It can be used to avoid `msg.value` stuck in the contract
///  if an upgrade doesn't perform an initialization call.
function _checkNonPayable() private {
    if (msg.value > 0) {
        revert ERC1967NonPayable();
    }
}
```

### onlyProxy()

- **Kind**: modifier
- **Source**: 2624:62:14
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/proxy/utils/UUPSUpgradeable.sol:UUPSUpgradeable:onlyProxy()`

```solidity
///  @dev Check that the execution is being performed through a delegatecall call and that the execution context is
///  a proxy contract with an implementation (as defined in ERC-1967) pointing to self. This should only be the case
///  for UUPS and transparent proxies that are using the current contract as their implementation. Execution of a
///  function through ERC-1167 minimal proxies (clones) would not normally pass this test, but is not guaranteed to
///  fail.
modifier onlyProxy() {
    _checkProxy();
    _;
}
```

### _checkProxy()

- **Kind**: internal
- **Source**: 4578:312:14
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/proxy/utils/UUPSUpgradeable.sol:UUPSUpgradeable:_checkProxy()`

```solidity
///  @dev Reverts if the execution is not performed via delegatecall or the execution
///  context is not of a proxy with an ERC-1967 compliant implementation pointing to self.
function _checkProxy() virtual internal view {
    if ((address(this) == __self) || (ERC1967Utils.getImplementation() != __self)) {
        revert UUPSUnauthorizedCallContext();
    }
}
```

### getImplementation()

- **Kind**: internal
- **Source**: 1441:138:35
- **Link**: `lib/openzeppelin-contracts/contracts/proxy/ERC1967/ERC1967Utils.sol:ERC1967Utils:getImplementation()`

```solidity
///  @dev Returns the current implementation address.
function getImplementation() internal view returns (address) {
    return StorageSlot.getAddressSlot(IMPLEMENTATION_SLOT).value;
}
```

## State Variable Reads

- **__self** (`address`)
- **IMPLEMENTATION_SLOT** (`bytes32`)

## State Variable Writes

- **IMPLEMENTATION_SLOT** (`bytes32`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: UUPSUpgradeable.upgradeToAndCall(address,bytes) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
  ├─ [1] ⚙️ FUNCTION: Auth._authorizeUpgrade(address) (NodeID: 1)
  │   💬 Args: [newImplementation]
  │   👁️  Def: internal
  │ └─ [2] 🔒 MODIFIER: AccessControlUpgradeable.onlyRole(bytes32) (NodeID: 2)
  │     💬 Args: [DEFAULT_ADMIN_ROLE]
  │   └─ [3] ⚙️ FUNCTION: AccessControlUpgradeable._checkRole(bytes32) (NodeID: 3)
  │       💬 Args: [DEFAULT_ADMIN_ROLE]
  │       👁️  Def: internal
  │     └─ [4] ⚙️ FUNCTION: AccessControlUpgradeable._checkRole(bytes32,address) (NodeID: 4)
  │         💬 Args: [role, _msgSender()]
  │         👁️  Def: internal
  │       ├─ [5] ⚙️ FUNCTION: ContextUpgradeable._msgSender() (NodeID: 7)
  │       │   💬 Args: [no args]
  │       │   👁️  Def: internal
  │       └─ [5] ⚙️ FUNCTION: AccessControlUpgradeable.hasRole(bytes32,address) (NodeID: 5)
  │           💬 Args: [role, account]
  │           👁️  Def: public
  │         └─ [6] ⚙️ FUNCTION: AccessControlUpgradeable._getAccessControlStorage() (NodeID: 6)
  │             💬 Args: [no args]
  │             👁️  Def: private
  ├─ [1] ⚙️ FUNCTION: UUPSUpgradeable._upgradeToAndCallUUPS(address,bytes) (NodeID: 8)
  │   💬 Args: [newImplementation, data]
  │   👁️  Def: private
  │ └─ [2] ⚙️ FUNCTION: ERC1967Utils.upgradeToAndCall(address,bytes) (NodeID: 9)
  │     💬 Args: [newImplementation, data]
  │     👁️  Def: internal
  │   ├─ [3] ⚙️ FUNCTION: ERC1967Utils._setImplementation(address) (NodeID: 10)
  │   │   💬 Args: [newImplementation]
  │   │   👁️  Def: private
  │   │ └─ [4] ⚙️ FUNCTION: StorageSlot.getAddressSlot(bytes32) (NodeID: 11)
  │   │     💬 Args: [IMPLEMENTATION_SLOT]
  │   │     👁️  Def: internal
  │   ├─ [3] ⚙️ FUNCTION: Address.functionDelegateCall(address,bytes) (NodeID: 12)
  │   │   💬 Args: [newImplementation, data]
  │   │   👁️  Def: internal
  │   │ └─ [4] ⚙️ FUNCTION: Address.verifyCallResultFromTarget(address,bool,bytes) (NodeID: 13)
  │   │     💬 Args: [target, success, returndata]
  │   │     👁️  Def: internal
  │   │   └─ [5] ⚙️ FUNCTION: Address._revert(bytes) (NodeID: 14)
  │   │       💬 Args: [returndata]
  │   │       👁️  Def: private
  │   └─ [3] ⚙️ FUNCTION: ERC1967Utils._checkNonPayable() (NodeID: 15)
  │       💬 Args: [no args]
  │       👁️  Def: private
  └─ [1] 🔒 MODIFIER: UUPSUpgradeable.onlyProxy() (NodeID: 16)
      💬 Args: [no args]
    └─ [2] ⚙️ FUNCTION: UUPSUpgradeable._checkProxy() (NodeID: 17)
        💬 Args: [no args]
        👁️  Def: internal
      └─ [3] ⚙️ FUNCTION: ERC1967Utils.getImplementation() (NodeID: 18)
          💬 Args: [no args]
          👁️  Def: internal
        └─ [4] ⚙️ FUNCTION: StorageSlot.getAddressSlot(bytes32) (NodeID: 19)
            💬 Args: [IMPLEMENTATION_SLOT]
            👁️  Def: internal
```

## Documentation

### Function Documentation

 @dev Upgrade the implementation of the proxy to `newImplementation`, and subsequently execute the function call
 encoded in `data`.
 Calls {_authorizeUpgrade}.
 Emits an {Upgraded} event.
 @custom:oz-upgrades-unsafe-allow-reachable delegatecall
