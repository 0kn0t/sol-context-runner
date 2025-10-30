# Contract: MultiSendCallOnly

## Metadata

- **Name**: MultiSendCallOnly
- **Type**: Contract
- **Path**: lib/safe-utils/lib/safe-smart-account/contracts/libraries/MultiSendCallOnly.sol
- **Documentation**:  @title Multi Send Call Only - Allows to batch multiple transactions into one, but only calls
   @notice The guard logic is not required here as this contract doesn't support nested delegate calls
   @author Stefan George - @Georgi87
   @author Richard Meissner - @rmeissner

## Public/External Functions

### multiSend(bytes)

- **Signature**: `multiSend(bytes)`
- **Visibility**: public
- **Source Range**: 1348:2069:116
- **Details**: [function_multiSend_bytes.md](./function_multiSend_bytes.md)

**Signature:**
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
function multiSend(bytes memory transactions) public payable;
```
