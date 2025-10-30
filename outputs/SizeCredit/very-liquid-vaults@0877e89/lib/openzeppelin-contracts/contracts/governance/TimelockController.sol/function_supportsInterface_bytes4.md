# Function: supportsInterface(bytes4)

**Contract**: [lib/openzeppelin-contracts/contracts/governance/TimelockController.sol/contract_TimelockController.md]

## Metadata

- **Contract**: TimelockController
- **Signature**: `supportsInterface(bytes4)`
- **Visibility**: public
- **Source Range**: 5653:195:72

## Implementation

```solidity
///  @dev See {IERC165-supportsInterface}.
function supportsInterface(bytes4 interfaceId) virtual override(AccessControl, ERC1155Holder) public view returns (bool) {
    return super.supportsInterface(interfaceId);
}
```

## Related Implementations

### supportsInterface(bytes4)

- **Kind**: internal
- **Source**: 650:221:86
- **Link**: `lib/openzeppelin-contracts/contracts/token/ERC1155/utils/ERC1155Holder.sol:ERC1155Holder:supportsInterface(bytes4)`

```solidity
///  @dev See {IERC165-supportsInterface}.
function supportsInterface(bytes4 interfaceId) virtual override(ERC165, IERC165) public view returns (bool) {
    return (interfaceId == type(IERC1155Receiver).interfaceId) || super.supportsInterface(interfaceId);
}
```

### supportsInterface(bytes4)

- **Kind**: internal
- **Source**: 2565:202:68
- **Link**: `lib/openzeppelin-contracts/contracts/access/AccessControl.sol:AccessControl:supportsInterface(bytes4)`

```solidity
///  @dev See {IERC165-supportsInterface}.
function supportsInterface(bytes4 interfaceId) virtual override public view returns (bool) {
    return (interfaceId == type(IAccessControl).interfaceId) || super.supportsInterface(interfaceId);
}
```

### supportsInterface(bytes4)

- **Kind**: internal
- **Source**: 763:146:107
- **Link**: `lib/openzeppelin-contracts/contracts/utils/introspection/ERC165.sol:ERC165:supportsInterface(bytes4)`

```solidity
///  @dev See {IERC165-supportsInterface}.
function supportsInterface(bytes4 interfaceId) virtual public view returns (bool) {
    return interfaceId == type(IERC165).interfaceId;
}
```

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: TimelockController.supportsInterface(bytes4) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
  └─ [1] ⚙️ FUNCTION: ERC1155Holder.supportsInterface(bytes4) (NodeID: 1)
      💬 Args: [interfaceId]
      👁️  Def: public
    └─ [2] ⚙️ FUNCTION: AccessControl.supportsInterface(bytes4) (NodeID: 2)
        💬 Args: [interfaceId]
        👁️  Def: public
      └─ [3] ⚙️ FUNCTION: ERC165.supportsInterface(bytes4) (NodeID: 3)
          💬 Args: [interfaceId]
          👁️  Def: public
```

## Documentation

### Function Documentation

 @dev See {IERC165-supportsInterface}.

### Interface Documentation

 @dev Returns true if this contract implements the interface defined by
 `interfaceId`. See the corresponding
 https://eips.ethereum.org/EIPS/eip-165#how-interfaces-are-identified[ERC section]
 to learn more about how these ids are created.
 This function call must use less than 30 000 gas.
