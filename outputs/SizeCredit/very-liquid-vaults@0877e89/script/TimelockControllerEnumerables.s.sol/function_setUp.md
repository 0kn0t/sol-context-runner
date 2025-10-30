# Function: setUp()

**Contract**: [script/TimelockControllerEnumerables.s.sol/contract_TimelockControllerEnumerablesScript.md]

## Metadata

- **Contract**: TimelockControllerEnumerablesScript
- **Signature**: `setUp()`
- **Visibility**: public
- **Source Range**: 475:270:139

## Implementation

```solidity
function setUp() public {
    adminMultisig = vm.envAddress("ADMIN_MULTISIG");
    vaultManagerMultisig = vm.envAddress("VAULT_MANAGER_MULTISIG");
    guardians = vm.envAddress("GUARDIANS", ",");
    strategists = vm.envAddress("STRATEGISTS", ",");
}
```

## External Calls

- **Vm::envAddress(string)**
- **Vm::envAddress(string,string)**

## State Variable Writes

- **adminMultisig** (`address`)
- **vaultManagerMultisig** (`address`)
- **guardians** (`address[]`)
- **strategists** (`address[]`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: TimelockControllerEnumerablesScript.setUp() (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
```
