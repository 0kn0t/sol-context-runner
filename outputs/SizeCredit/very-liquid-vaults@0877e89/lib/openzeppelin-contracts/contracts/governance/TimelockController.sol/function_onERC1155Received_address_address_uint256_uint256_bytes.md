# Function: onERC1155Received(address,address,uint256,uint256,bytes)

**Contract**: [lib/openzeppelin-contracts/contracts/governance/TimelockController.sol/contract_TimelockController.md]

## Metadata

- **Contract**: TimelockController
- **Signature**: `onERC1155Received(address,address,uint256,uint256,bytes)`
- **Visibility**: public
- **Source Range**: 877:219:86
- **Inherited From**: ERC1155Holder

## Implementation

```solidity
function onERC1155Received(address, address, uint256, uint256, bytes memory) virtual override public returns (bytes4) {
    return this.onERC1155Received.selector;
}
```

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: ERC1155Holder.onERC1155Received(address,address,uint256,uint256,bytes) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
```

## Documentation

### Interface Documentation

 @dev Handles the receipt of a single ERC-1155 token type. This function is
 called at the end of a `safeTransferFrom` after the balance has been updated.
 NOTE: To accept the transfer, this must return
 `bytes4(keccak256("onERC1155Received(address,address,uint256,uint256,bytes)"))`
 (i.e. 0xf23a6e61, or its own function selector).
 @param operator The address which initiated the transfer (i.e. msg.sender)
 @param from The address which previously owned the token
 @param id The ID of the token being transferred
 @param value The amount of tokens being transferred
 @param data Additional data with no specified format
 @return `bytes4(keccak256("onERC1155Received(address,address,uint256,uint256,bytes)"))` if transfer is allowed
