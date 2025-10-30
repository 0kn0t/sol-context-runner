# Function: onERC721Received(address,address,uint256,bytes)

**Contract**: [lib/openzeppelin-community-contracts/contracts/governance/TimelockControllerEnumerable.sol/contract_TimelockControllerEnumerable.md]

## Metadata

- **Contract**: TimelockControllerEnumerable
- **Signature**: `onERC721Received(address,address,uint256,bytes)`
- **Visibility**: public
- **Source Range**: 639:153:94
- **Inherited From**: ERC721Holder

## Implementation

```solidity
///  @dev See {IERC721Receiver-onERC721Received}.
///  Always returns `IERC721Receiver.onERC721Received.selector`.
function onERC721Received(address, address, uint256, bytes memory) virtual public returns (bytes4) {
    return this.onERC721Received.selector;
}
```

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: ERC721Holder.onERC721Received(address,address,uint256,bytes) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
```

## Documentation

### Function Documentation

 @dev See {IERC721Receiver-onERC721Received}.
 Always returns `IERC721Receiver.onERC721Received.selector`.

### Interface Documentation

 @dev Whenever an {IERC721} `tokenId` token is transferred to this contract via {IERC721-safeTransferFrom}
 by `operator` from `from`, this function is called.
 It must return its Solidity selector to confirm the token transfer.
 If any other value is returned or the interface is not implemented by the recipient, the transfer will be
 reverted.
 The selector can be obtained in Solidity with `IERC721Receiver.onERC721Received.selector`.
