# Function: nonces(address)

**Contract**: [lib/aave-v3-origin/src/contracts/instances/VariableDebtTokenInstance.sol/contract_VariableDebtTokenInstance.md]

## Metadata

- **Contract**: VariableDebtTokenInstance
- **Signature**: `nonces(address)`
- **Visibility**: public
- **Source Range**: 1255:101:34
- **Inherited From**: EIP712Base

## Implementation

```solidity
///  @notice Returns the nonce value for address specified as parameter
///  @param owner The address for which the nonce is being returned
///  @return The nonce value for the input address`
function nonces(address owner) virtual public view returns (uint256) {
    return _nonces[owner];
}
```

## State Variable Reads

- **_nonces** (`mapping(address => uint256)`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: EIP712Base.nonces(address) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
```

## Documentation

### Function Documentation

 @notice Returns the nonce value for address specified as parameter
 @param owner The address for which the nonce is being returned
 @return The nonce value for the input address`
