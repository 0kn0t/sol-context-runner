# Interface: Authority

## Metadata

- **Name**: Authority
- **Type**: Interface
- **Path**: src/Dependencies/Authority.sol
- **Documentation**: @notice A generic interface for a contract which provides authorization data to an Auth instance.
   @author Solmate (https://github.com/transmissions11/solmate/blob/main/src/auth/Auth.sol)
   @author Modified from Dappsys (https://github.com/dapphub/ds-auth/blob/master/src/auth.sol)

## Public/External Functions

### canCall(address,address,bytes4)

- **Signature**: `canCall(address,address,bytes4)`
- **Visibility**: external
- **Source Range**: 384:96:26

**Signature:**
```solidity
function canCall(address user, address target, bytes4 functionSig) external view returns (bool);;
```
