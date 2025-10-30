# Function: setUp()

**Contract**: [script/BaseScript.s.sol/contract_BaseScript.md]

## Metadata

- **Contract**: BaseScript
- **Signature**: `setUp()`
- **Visibility**: public
- **Source Range**: 416:190:125

## Implementation

```solidity
function setUp() virtual public {
    create2Deployer = ICreate2Deployer(0x13b0D85CcB8bf860b6b79AF3029fCA081AE9beF2);
    vm.label(address(create2Deployer), "Create2Deployer");
}
```

## External Calls

- **Vm::label(address,string)**

## State Variable Reads

- **create2Deployer** (`contract ICreate2Deployer`) [script/ICreate2Deployer.s.sol/interface_ICreate2Deployer.md]

## State Variable Writes

- **create2Deployer** (`contract ICreate2Deployer`) [script/ICreate2Deployer.s.sol/interface_ICreate2Deployer.md]

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: BaseScript.setUp() (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
```
