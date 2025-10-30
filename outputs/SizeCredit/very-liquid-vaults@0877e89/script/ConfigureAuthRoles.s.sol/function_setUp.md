# Function: setUp()

**Contract**: [script/ConfigureAuthRoles.s.sol/contract_ConfigureAuthRolesScript.md]

## Metadata

- **Contract**: ConfigureAuthRolesScript
- **Signature**: `setUp()`
- **Visibility**: public
- **Source Range**: 620:497:128

## Implementation

```solidity
function setUp() public {
    auth = Auth(vm.envAddress("AUTH"));
    timelockController_DEFAULT_ADMIN_ROLE = TimelockControllerEnumerable(payable(vm.envAddress("TIMELOCK_CONTROLLER_DEFAULT_ADMIN_ROLE")));
    timelockController_VAULT_MANAGER_ROLE = TimelockControllerEnumerable(payable(vm.envAddress("TIMELOCK_CONTROLLER_VAULT_MANAGER_ROLE")));
    guardians = vm.envAddress("GUARDIANS", ",");
    strategists = vm.envAddress("STRATEGISTS", ",");
}
```

## External Calls

- **Vm::envAddress(string)**
- **Vm::envAddress(string,string)**

## State Variable Writes

- **auth** (`contract Auth`) [src/Auth.sol/contract_Auth.md]
- **timelockController_DEFAULT_ADMIN_ROLE** (`contract TimelockControllerEnumerable`) [lib/openzeppelin-community-contracts/contracts/governance/TimelockControllerEnumerable.sol/contract_TimelockControllerEnumerable.md]
- **timelockController_VAULT_MANAGER_ROLE** (`contract TimelockControllerEnumerable`) [lib/openzeppelin-community-contracts/contracts/governance/TimelockControllerEnumerable.sol/contract_TimelockControllerEnumerable.md]
- **guardians** (`address[]`)
- **strategists** (`address[]`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: ConfigureAuthRolesScript.setUp() (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
```
