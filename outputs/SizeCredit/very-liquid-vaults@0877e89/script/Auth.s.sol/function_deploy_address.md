# Function: deploy(address)

**Contract**: [script/Auth.s.sol/contract_AuthScript.md]

## Metadata

- **Contract**: AuthScript
- **Signature**: `deploy(address)`
- **Visibility**: public
- **Source Range**: 513:178:124

## Implementation

```solidity
function deploy(address admin_) public returns (Auth auth) {
    return Auth(address(new ERC1967Proxy(address(new Auth()), abi.encodeCall(Auth.initialize, (admin_)))));
}
```

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: AuthScript.deploy(address) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
```
