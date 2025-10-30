# Function: multiSend(bytes)

**Contract**: [lib/safe-utils/lib/safe-smart-account/contracts/libraries/MultiSendCallOnly.sol/contract_MultiSendCallOnly.md]

## Metadata

- **Contract**: MultiSendCallOnly
- **Signature**: `multiSend(bytes)`
- **Visibility**: public
- **Source Range**: 1348:2069:116

## Implementation

```solidity
///  @dev Sends multiple transactions and reverts all if one fails.
///  @param transactions Encoded transactions. Each transaction is encoded as a packed bytes of
///                      operation has to be uint8(0) in this version (=> 1 byte),
///                      to as a address (=> 20 bytes),
///                      value as a uint256 (=> 32 bytes),
///                      data length as a uint256 (=> 32 bytes),
///                      data as bytes.
///                      see abi.encodePacked for more information on packed encoding
///  @notice The code is for most part the same as the normal MultiSend (to keep compatibility),
///          but reverts if a transaction tries to use a delegatecall.
///  @notice This method is payable as delegatecalls keep the msg.value from the previous call
///          If the calling method (e.g. execTransaction) received ETH this would revert otherwise
function multiSend(bytes memory transactions) public payable {
    assembly {
        let length := mload(transactions)
        let i := 0x20
        for {} lt(i, length) {} {
            let operation := shr(0xf8, mload(add(transactions, i)))
            let to := shr(0x60, mload(add(transactions, add(i, 0x01))))
            let value := mload(add(transactions, add(i, 0x15)))
            let dataLength := mload(add(transactions, add(i, 0x35)))
            let data := add(transactions, add(i, 0x55))
            let success := 0
            switch operation
            case 0 {
                success := call(gas(), to, value, data, dataLength, 0, 0)
            }
            case 1 {
                revert(0, 0)
            }
            if eq(success, 0) {
                revert(0, 0)
            }
            i := add(i, add(0x55, dataLength))
        }
    }
}
```

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: MultiSendCallOnly.multiSend(bytes) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
```

## Documentation

### Function Documentation

 @dev Sends multiple transactions and reverts all if one fails.
 @param transactions Encoded transactions. Each transaction is encoded as a packed bytes of
                     operation has to be uint8(0) in this version (=> 1 byte),
                     to as a address (=> 20 bytes),
                     value as a uint256 (=> 32 bytes),
                     data length as a uint256 (=> 32 bytes),
                     data as bytes.
                     see abi.encodePacked for more information on packed encoding
 @notice The code is for most part the same as the normal MultiSend (to keep compatibility),
         but reverts if a transaction tries to use a delegatecall.
 @notice This method is payable as delegatecalls keep the msg.value from the previous call
         If the calling method (e.g. execTransaction) received ETH this would revert otherwise
