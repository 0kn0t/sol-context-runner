# Function: setUp()

**Contract**: [script/Auth.s.sol/contract_AuthScript.md]

## Metadata

- **Contract**: AuthScript
- **Signature**: `setUp()`
- **Visibility**: public
- **Source Range**: 289:71:124

## Implementation

```solidity
function setUp() public {
    admin = vm.envAddress("ADMIN");
}
```

## External Calls

- **Vm::envAddress(string)**

## State Variable Writes

- **admin** (`address`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: AuthScript.setUp() (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
```
