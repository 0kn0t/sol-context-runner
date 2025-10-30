# Function: upgradeToAndCall(address,bytes)

**Contract**: [src/strategies/ERC4626StrategyVault.sol/contract_ERC4626StrategyVault.md]

## Metadata

- **Contract**: ERC4626StrategyVault
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
- **Source**: 5388:103:56
- **Link**: `src/BaseVault.sol:BaseVault:_authorizeUpgrade(address)`

```solidity
/// @notice Authorizes contract upgrades
///  @dev Only addresses with DEFAULT_ADMIN_ROLE can authorize upgrades
function _authorizeUpgrade(address newImplementation) override internal onlyAuth(DEFAULT_ADMIN_ROLE) {}
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

- **auth** (`contract Auth`) [src/utils/Auth.sol/contract_Auth.md]
- **__self** (`address`)
- **IMPLEMENTATION_SLOT** (`bytes32`)

## State Variable Writes

- **IMPLEMENTATION_SLOT** (`bytes32`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: UUPSUpgradeable.upgradeToAndCall(address,bytes) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
  ├─ [1] ⚙️ FUNCTION: BaseVault._authorizeUpgrade(address) (NodeID: 1)
  │   💬 Args: [newImplementation]
  │   👁️  Def: internal
  │ └─ [2] 🔒 MODIFIER: BaseVault.onlyAuth(bytes32) (NodeID: 2)
  │     💬 Args: [DEFAULT_ADMIN_ROLE]
  ├─ [1] ⚙️ FUNCTION: UUPSUpgradeable._upgradeToAndCallUUPS(address,bytes) (NodeID: 3)
  │   💬 Args: [newImplementation, data]
  │   👁️  Def: private
  │ └─ [2] ⚙️ FUNCTION: ERC1967Utils.upgradeToAndCall(address,bytes) (NodeID: 4)
  │     💬 Args: [newImplementation, data]
  │     👁️  Def: internal
  │   ├─ [3] ⚙️ FUNCTION: ERC1967Utils._setImplementation(address) (NodeID: 5)
  │   │   💬 Args: [newImplementation]
  │   │   👁️  Def: private
  │   │ └─ [4] ⚙️ FUNCTION: StorageSlot.getAddressSlot(bytes32) (NodeID: 6)
  │   │     💬 Args: [IMPLEMENTATION_SLOT]
  │   │     👁️  Def: internal
  │   ├─ [3] ⚙️ FUNCTION: Address.functionDelegateCall(address,bytes) (NodeID: 7)
  │   │   💬 Args: [newImplementation, data]
  │   │   👁️  Def: internal
  │   │ └─ [4] ⚙️ FUNCTION: Address.verifyCallResultFromTarget(address,bool,bytes) (NodeID: 8)
  │   │     💬 Args: [target, success, returndata]
  │   │     👁️  Def: internal
  │   │   └─ [5] ⚙️ FUNCTION: Address._revert(bytes) (NodeID: 9)
  │   │       💬 Args: [returndata]
  │   │       👁️  Def: private
  │   └─ [3] ⚙️ FUNCTION: ERC1967Utils._checkNonPayable() (NodeID: 10)
  │       💬 Args: [no args]
  │       👁️  Def: private
  └─ [1] 🔒 MODIFIER: UUPSUpgradeable.onlyProxy() (NodeID: 11)
      💬 Args: [no args]
    └─ [2] ⚙️ FUNCTION: UUPSUpgradeable._checkProxy() (NodeID: 12)
        💬 Args: [no args]
        👁️  Def: internal
      └─ [3] ⚙️ FUNCTION: ERC1967Utils.getImplementation() (NodeID: 13)
          💬 Args: [no args]
          👁️  Def: internal
        └─ [4] ⚙️ FUNCTION: StorageSlot.getAddressSlot(bytes32) (NodeID: 14)
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
