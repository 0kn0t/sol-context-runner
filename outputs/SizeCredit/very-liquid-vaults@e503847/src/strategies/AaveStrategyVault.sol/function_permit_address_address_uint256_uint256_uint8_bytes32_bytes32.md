# Function: permit(address,address,uint256,uint256,uint8,bytes32,bytes32)

**Contract**: [src/strategies/AaveStrategyVault.sol/contract_AaveStrategyVault.md]

## Metadata

- **Contract**: AaveStrategyVault
- **Signature**: `permit(address,address,uint256,uint256,uint8,bytes32,bytes32)`
- **Visibility**: public
- **Source Range**: 2098:672:16
- **Inherited From**: ERC20PermitUpgradeable

## Implementation

```solidity
///  @inheritdoc IERC20Permit
function permit(address owner, address spender, uint256 value, uint256 deadline, uint8 v, bytes32 r, bytes32 s) virtual public {
    if (block.timestamp > deadline) {
        revert ERC2612ExpiredSignature(deadline);
    }
    bytes32 structHash = keccak256(abi.encode(PERMIT_TYPEHASH, owner, spender, value, _useNonce(owner), deadline));
    bytes32 hash = _hashTypedDataV4(structHash);
    address signer = ECDSA.recover(hash, v, r, s);
    if (signer != owner) {
        revert ERC2612InvalidSigner(signer, owner);
    }
    _approve(owner, spender, value);
}
```

## Related Implementations

### _useNonce(address)

- **Kind**: internal
- **Source**: 1537:452:20
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/utils/NoncesUpgradeable.sol:NoncesUpgradeable:_useNonce(address)`

```solidity
///  @dev Consumes a nonce.
///  Returns the current value and increments nonce.
function _useNonce(address owner) virtual internal returns (uint256) {
    NoncesStorage storage $ = _getNoncesStorage();
    unchecked {
        return $._nonces[owner]++;
    }
}
```

### _getNoncesStorage()

- **Kind**: internal
- **Source**: 886:156:20
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/utils/NoncesUpgradeable.sol:NoncesUpgradeable:_getNoncesStorage()`

```solidity
function _getNoncesStorage() private pure returns (NoncesStorage storage $) {
    assembly {
        $.slot := NoncesStorageLocation
    }
}
```

### _hashTypedDataV4(bytes32)

- **Kind**: internal
- **Source**: 4946:176:23
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/utils/cryptography/EIP712Upgradeable.sol:EIP712Upgradeable:_hashTypedDataV4(bytes32)`

```solidity
///  @dev Given an already https://eips.ethereum.org/EIPS/eip-712#definition-of-hashstruct[hashed struct], this
///  function returns the hash of the fully encoded EIP712 message for this domain.
///  This hash can be used together with {ECDSA-recover} to obtain the signer of a message. For example:
///  ```solidity
///  bytes32 digest = _hashTypedDataV4(keccak256(abi.encode(
///      keccak256("Mail(address to,string contents)"),
///      mailTo,
///      keccak256(bytes(mailContents))
///  )));
///  address signer = ECDSA.recover(digest, signature);
///  ```
function _hashTypedDataV4(bytes32 structHash) virtual internal view returns (bytes32) {
    return MessageHashUtils.toTypedDataHash(_domainSeparatorV4(), structHash);
}
```

### toTypedDataHash(bytes32,bytes32)

- **Kind**: internal
- **Source**: 3874:374:50
- **Link**: `lib/openzeppelin-contracts/contracts/utils/cryptography/MessageHashUtils.sol:MessageHashUtils:toTypedDataHash(bytes32,bytes32)`

```solidity
///  @dev Returns the keccak256 digest of an EIP-712 typed data (ERC-191 version `0x01`).
///  The digest is calculated from a `domainSeparator` and a `structHash`, by prefixing them with
///  `\x19\x01` and hashing the result. It corresponds to the hash signed by the
///  https://eips.ethereum.org/EIPS/eip-712[`eth_signTypedData`] JSON-RPC method as part of EIP-712.
///  See {ECDSA-recover}.
function toTypedDataHash(bytes32 domainSeparator, bytes32 structHash) internal pure returns (bytes32 digest) {
    assembly ("memory-safe") {
        let ptr := mload(0x40)
        mstore(ptr, "\u0019\u0001")
        mstore(add(ptr, 0x02), domainSeparator)
        mstore(add(ptr, 0x22), structHash)
        digest := keccak256(ptr, 0x42)
    }
}
```

### _domainSeparatorV4()

- **Kind**: internal
- **Source**: 4015:109:23
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/utils/cryptography/EIP712Upgradeable.sol:EIP712Upgradeable:_domainSeparatorV4()`

```solidity
///  @dev Returns the domain separator for the current chain.
function _domainSeparatorV4() internal view returns (bytes32) {
    return _buildDomainSeparator();
}
```

### _buildDomainSeparator()

- **Kind**: internal
- **Source**: 4130:191:23
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/utils/cryptography/EIP712Upgradeable.sol:EIP712Upgradeable:_buildDomainSeparator()`

```solidity
function _buildDomainSeparator() private view returns (bytes32) {
    return keccak256(abi.encode(TYPE_HASH, _EIP712NameHash(), _EIP712VersionHash(), block.chainid, address(this)));
}
```

### _EIP712NameHash()

- **Kind**: internal
- **Source**: 7057:687:23
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/utils/cryptography/EIP712Upgradeable.sol:EIP712Upgradeable:_EIP712NameHash()`

```solidity
///  @dev The hash of the name parameter for the EIP712 domain.
///  NOTE: In previous versions this function was virtual. In this version you should override `_EIP712Name` instead.
function _EIP712NameHash() internal view returns (bytes32) {
    EIP712Storage storage $ = _getEIP712Storage();
    string memory name = _EIP712Name();
    if (bytes(name).length > 0) {
        return keccak256(bytes(name));
    } else {
        bytes32 hashedName = $._hashedName;
        if (hashedName != 0) {
            return hashedName;
        } else {
            return keccak256("");
        }
    }
}
```

### _getEIP712Storage()

- **Kind**: internal
- **Source**: 2720:156:23
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/utils/cryptography/EIP712Upgradeable.sol:EIP712Upgradeable:_getEIP712Storage()`

```solidity
function _getEIP712Storage() private pure returns (EIP712Storage storage $) {
    assembly {
        $.slot := EIP712StorageLocation
    }
}
```

### _EIP712Name()

- **Kind**: internal
- **Source**: 6299:155:23
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/utils/cryptography/EIP712Upgradeable.sol:EIP712Upgradeable:_EIP712Name()`

```solidity
///  @dev The name parameter for the EIP712 domain.
///  NOTE: This function reads from storage by default, but can be redefined to return a constant value if gas costs
///  are a concern.
function _EIP712Name() virtual internal view returns (string memory) {
    EIP712Storage storage $ = _getEIP712Storage();
    return $._name;
}
```

### _EIP712VersionHash()

- **Kind**: internal
- **Source**: 7965:723:23
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/utils/cryptography/EIP712Upgradeable.sol:EIP712Upgradeable:_EIP712VersionHash()`

```solidity
///  @dev The hash of the version parameter for the EIP712 domain.
///  NOTE: In previous versions this function was virtual. In this version you should override `_EIP712Version` instead.
function _EIP712VersionHash() internal view returns (bytes32) {
    EIP712Storage storage $ = _getEIP712Storage();
    string memory version = _EIP712Version();
    if (bytes(version).length > 0) {
        return keccak256(bytes(version));
    } else {
        bytes32 hashedVersion = $._hashedVersion;
        if (hashedVersion != 0) {
            return hashedVersion;
        } else {
            return keccak256("");
        }
    }
}
```

### _EIP712Version()

- **Kind**: internal
- **Source**: 6681:161:23
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/utils/cryptography/EIP712Upgradeable.sol:EIP712Upgradeable:_EIP712Version()`

```solidity
///  @dev The version parameter for the EIP712 domain.
///  NOTE: This function reads from storage by default, but can be redefined to return a constant value if gas costs
///  are a concern.
function _EIP712Version() virtual internal view returns (string memory) {
    EIP712Storage storage $ = _getEIP712Storage();
    return $._version;
}
```

### recover(bytes32,uint8,bytes32,bytes32)

- **Kind**: internal
- **Source**: 6887:260:49
- **Link**: `lib/openzeppelin-contracts/contracts/utils/cryptography/ECDSA.sol:ECDSA:recover(bytes32,uint8,bytes32,bytes32)`

```solidity
///  @dev Overload of {ECDSA-recover} that receives the `v`,
///  `r` and `s` signature fields separately.
function recover(bytes32 hash, uint8 v, bytes32 r, bytes32 s) internal pure returns (address) {
    (address recovered, RecoverError error, bytes32 errorArg) = tryRecover(hash, v, r, s);
    _throwError(error, errorArg);
    return recovered;
}
```

### tryRecover(bytes32,uint8,bytes32,bytes32)

- **Kind**: internal
- **Source**: 5203:1551:49
- **Link**: `lib/openzeppelin-contracts/contracts/utils/cryptography/ECDSA.sol:ECDSA:tryRecover(bytes32,uint8,bytes32,bytes32)`

```solidity
///  @dev Overload of {ECDSA-tryRecover} that receives the `v`,
///  `r` and `s` signature fields separately.
function tryRecover(bytes32 hash, uint8 v, bytes32 r, bytes32 s) internal pure returns (address recovered, RecoverError err, bytes32 errArg) {
    if (uint256(s) > 0x7FFFFFFFFFFFFFFFFFFFFFFFFFFFFFFF5D576E7357A4501DDFE92F46681B20A0) {
        return (address(0), RecoverError.InvalidSignatureS, s);
    }
    address signer = ecrecover(hash, v, r, s);
    if (signer == address(0)) {
        return (address(0), RecoverError.InvalidSignature, bytes32(0));
    }
    return (signer, RecoverError.NoError, bytes32(0));
}
```

### _throwError(enum ECDSA.RecoverError,bytes32)

- **Kind**: internal
- **Source**: 7280:532:49
- **Link**: `lib/openzeppelin-contracts/contracts/utils/cryptography/ECDSA.sol:ECDSA:_throwError(enum ECDSA.RecoverError,bytes32)`

```solidity
///  @dev Optionally reverts with the corresponding custom error according to the `error` argument provided.
function _throwError(RecoverError error, bytes32 errorArg) private pure {
    if (error == RecoverError.NoError) {
        return;
    } else if (error == RecoverError.InvalidSignature) {
        revert ECDSAInvalidSignature();
    } else if (error == RecoverError.InvalidSignatureLength) {
        revert ECDSAInvalidSignatureLength(uint256(errorArg));
    } else if (error == RecoverError.InvalidSignatureS) {
        revert ECDSAInvalidSignatureS(errorArg);
    }
}
```

### _approve(address,address,uint256)

- **Kind**: internal
- **Source**: 9982:128:15
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/token/ERC20/ERC20Upgradeable.sol:ERC20Upgradeable:_approve(address,address,uint256)`

```solidity
///  @dev Sets `value` as the allowance of `spender` over the `owner`'s tokens.
///  This internal function is equivalent to `approve`, and can be used to
///  e.g. set automatic allowances for certain subsystems, etc.
///  Emits an {Approval} event.
///  Requirements:
///  - `owner` cannot be the zero address.
///  - `spender` cannot be the zero address.
///  Overrides to this logic should be done to the variant with an additional `bool emitEvent` argument.
function _approve(address owner, address spender, uint256 value) internal {
    _approve(owner, spender, value, true);
}
```

### _approve(address,address,uint256,bool)

- **Kind**: internal
- **Source**: 10957:487:15
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/token/ERC20/ERC20Upgradeable.sol:ERC20Upgradeable:_approve(address,address,uint256,bool)`

```solidity
///  @dev Variant of {_approve} with an optional flag to enable or disable the {Approval} event.
///  By default (when calling {_approve}) the flag is set to true. On the other hand, approval changes made by
///  `_spendAllowance` during the `transferFrom` operation set the flag to false. This saves gas by not emitting any
///  `Approval` event during `transferFrom` operations.
///  Anyone who wishes to continue emitting `Approval` events on the`transferFrom` operation can force the flag to
///  true using the following override:
///  ```solidity
///  function _approve(address owner, address spender, uint256 value, bool) internal virtual override {
///      super._approve(owner, spender, value, true);
///  }
///  ```
///  Requirements are the same as {_approve}.
function _approve(address owner, address spender, uint256 value, bool emitEvent) virtual internal {
    ERC20Storage storage $ = _getERC20Storage();
    if (owner == address(0)) {
        revert ERC20InvalidApprover(address(0));
    }
    if (spender == address(0)) {
        revert ERC20InvalidSpender(address(0));
    }
    $._allowances[owner][spender] = value;
    if (emitEvent) {
        emit Approval(owner, spender, value);
    }
}
```

### _getERC20Storage()

- **Kind**: internal
- **Source**: 1947:153:15
- **Link**: `lib/openzeppelin-contracts-upgradeable/contracts/token/ERC20/ERC20Upgradeable.sol:ERC20Upgradeable:_getERC20Storage()`

```solidity
function _getERC20Storage() private pure returns (ERC20Storage storage $) {
    assembly {
        $.slot := ERC20StorageLocation
    }
}
```

## State Variable Reads

- **PERMIT_TYPEHASH** (`bytes32`)
- **TYPE_HASH** (`bytes32`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: ERC20PermitUpgradeable.permit(address,address,uint256,uint256,uint8,bytes32,bytes32) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
  ├─ [1] ⚙️ FUNCTION: NoncesUpgradeable._useNonce(address) (NodeID: 1)
  │   💬 Args: [owner]
  │   👁️  Def: internal
  │ └─ [2] ⚙️ FUNCTION: NoncesUpgradeable._getNoncesStorage() (NodeID: 2)
  │     💬 Args: [no args]
  │     👁️  Def: private
  ├─ [1] ⚙️ FUNCTION: EIP712Upgradeable._hashTypedDataV4(bytes32) (NodeID: 3)
  │   💬 Args: [structHash]
  │   👁️  Def: internal
  │ └─ [2] ⚙️ FUNCTION: MessageHashUtils.toTypedDataHash(bytes32,bytes32) (NodeID: 4)
  │     💬 Args: [_domainSeparatorV4(), structHash]
  │     👁️  Def: internal
  │   └─ [3] ⚙️ FUNCTION: EIP712Upgradeable._domainSeparatorV4() (NodeID: 5)
  │       💬 Args: [no args]
  │       👁️  Def: internal
  │     └─ [4] ⚙️ FUNCTION: EIP712Upgradeable._buildDomainSeparator() (NodeID: 6)
  │         💬 Args: [no args]
  │         👁️  Def: private
  │       ├─ [5] ⚙️ FUNCTION: EIP712Upgradeable._EIP712NameHash() (NodeID: 7)
  │       │   💬 Args: [no args]
  │       │   👁️  Def: internal
  │       │ ├─ [6] ⚙️ FUNCTION: EIP712Upgradeable._getEIP712Storage() (NodeID: 8)
  │       │ │   💬 Args: [no args]
  │       │ │   👁️  Def: private
  │       │ └─ [6] ⚙️ FUNCTION: EIP712Upgradeable._EIP712Name() (NodeID: 9)
  │       │     💬 Args: [no args]
  │       │     👁️  Def: internal
  │       │   └─ [7] ⚙️ FUNCTION: EIP712Upgradeable._getEIP712Storage() (NodeID: 10)
  │       │       💬 Args: [no args]
  │       │       👁️  Def: private
  │       └─ [5] ⚙️ FUNCTION: EIP712Upgradeable._EIP712VersionHash() (NodeID: 11)
  │           💬 Args: [no args]
  │           👁️  Def: internal
  │         ├─ [6] ⚙️ FUNCTION: EIP712Upgradeable._getEIP712Storage() (NodeID: 12)
  │         │   💬 Args: [no args]
  │         │   👁️  Def: private
  │         └─ [6] ⚙️ FUNCTION: EIP712Upgradeable._EIP712Version() (NodeID: 13)
  │             💬 Args: [no args]
  │             👁️  Def: internal
  │           └─ [7] ⚙️ FUNCTION: EIP712Upgradeable._getEIP712Storage() (NodeID: 14)
  │               💬 Args: [no args]
  │               👁️  Def: private
  ├─ [1] ⚙️ FUNCTION: ECDSA.recover(bytes32,uint8,bytes32,bytes32) (NodeID: 15)
  │   💬 Args: [hash, v, r, s]
  │   👁️  Def: internal
  │ ├─ [2] ⚙️ FUNCTION: ECDSA.tryRecover(bytes32,uint8,bytes32,bytes32) (NodeID: 16)
  │ │   💬 Args: [hash, v, r, s]
  │ │   👁️  Def: internal
  │ └─ [2] ⚙️ FUNCTION: ECDSA._throwError(enum ECDSA.RecoverError,bytes32) (NodeID: 17)
  │     💬 Args: [error, errorArg]
  │     👁️  Def: private
  └─ [1] ⚙️ FUNCTION: ERC20Upgradeable._approve(address,address,uint256) (NodeID: 18)
      💬 Args: [owner, spender, value]
      👁️  Def: internal
    └─ [2] ⚙️ FUNCTION: ERC20Upgradeable._approve(address,address,uint256,bool) (NodeID: 19)
        💬 Args: [owner, spender, value, true]
        👁️  Def: internal
      └─ [3] ⚙️ FUNCTION: ERC20Upgradeable._getERC20Storage() (NodeID: 20)
          💬 Args: [no args]
          👁️  Def: private
```

## Documentation

### Function Documentation

 @inheritdoc IERC20Permit

### Interface Documentation

 @dev Sets `value` as the allowance of `spender` over ``owner``'s tokens,
 given ``owner``'s signed approval.
 IMPORTANT: The same issues {IERC20-approve} has related to transaction
 ordering also apply here.
 Emits an {Approval} event.
 Requirements:
 - `spender` cannot be the zero address.
 - `deadline` must be a timestamp in the future.
 - `v`, `r` and `s` must be a valid `secp256k1` signature from `owner`
 over the EIP712-formatted function arguments.
 - the signature must use ``owner``'s current nonce (see {nonces}).
 For more information on the signature format, see the
 https://eips.ethereum.org/EIPS/eip-2612#specification[relevant EIP
 section].
 CAUTION: See Security Considerations above.
