# Function: onERC1155BatchReceived(address,address,uint256[],uint256[],bytes)

**Contract**: [lib/openzeppelin-community-contracts/contracts/governance/TimelockControllerEnumerable.sol/contract_TimelockControllerEnumerable.md]

## Metadata

- **Contract**: TimelockControllerEnumerable
- **Signature**: `onERC1155BatchReceived(address,address,uint256[],uint256[],bytes)`
- **Visibility**: public
- **Source Range**: 1102:247:86
- **Inherited From**: ERC1155Holder

## Implementation

```solidity
function onERC1155BatchReceived(address, address, uint256[] memory, uint256[] memory, bytes memory) virtual override public returns (bytes4) {
    return this.onERC1155BatchReceived.selector;
}
```

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: ERC1155Holder.onERC1155BatchReceived(address,address,uint256[],uint256[],bytes) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
```

## Documentation

### Interface Documentation

 @dev Handles the receipt of a multiple ERC-1155 token types. This function
 is called at the end of a `safeBatchTransferFrom` after the balances have
 been updated.
 NOTE: To accept the transfer(s), this must return
 `bytes4(keccak256("onERC1155BatchReceived(address,address,uint256[],uint256[],bytes)"))`
 (i.e. 0xbc197c81, or its own function selector).
 @param operator The address which initiated the batch transfer (i.e. msg.sender)
 @param from The address which previously owned the token
 @param ids An array containing ids of each token being transferred (order and length must match values array)
 @param values An array containing amounts of each token being transferred (order and length must match ids array)
 @param data Additional data with no specified format
 @return `bytes4(keccak256("onERC1155BatchReceived(address,address,uint256[],uint256[],bytes)"))` if transfer is allowed
