# Function: run()

**Contract**: [script/ConfigureAuthRoles.s.sol/contract_ConfigureAuthRolesScript.md]

## Metadata

- **Contract**: ConfigureAuthRolesScript
- **Signature**: `run()`
- **Visibility**: public
- **Source Range**: 1123:1102:128

## Implementation

```solidity
function run() public {
    vm.startBroadcast();
    require(auth.hasRole(DEFAULT_ADMIN_ROLE, msg.sender), "Deployer MUST have DEFAULT_ADMIN_ROLE to configure roles");
    auth.grantRole(DEFAULT_ADMIN_ROLE, address(timelockController_DEFAULT_ADMIN_ROLE));
    auth.grantRole(VAULT_MANAGER_ROLE, address(timelockController_VAULT_MANAGER_ROLE));
    for (uint256 i = 0; i < guardians.length; i++) {
        auth.grantRole(GUARDIAN_ROLE, guardians[i]);
    }
    for (uint256 i = 0; i < strategists.length; i++) {
        auth.grantRole(STRATEGIST_ROLE, strategists[i]);
    }
    auth.revokeRole(DEFAULT_ADMIN_ROLE, msg.sender);
    require(!auth.hasRole(DEFAULT_ADMIN_ROLE, msg.sender), "Deployer DOES NOT have DEFAULT_ADMIN_ROLE after configuration");
    vm.stopBroadcast();
}
```

## External Calls

- **Vm::startBroadcast()**
- **Auth::hasRole(bytes32,address)**
- **Auth::grantRole(bytes32,address)**
- **Auth::revokeRole(bytes32,address)**
- **Vm::stopBroadcast()**

## State Variable Reads

- **auth** (`contract Auth`) [src/Auth.sol/contract_Auth.md]
- **timelockController_DEFAULT_ADMIN_ROLE** (`contract TimelockControllerEnumerable`) [lib/openzeppelin-community-contracts/contracts/governance/TimelockControllerEnumerable.sol/contract_TimelockControllerEnumerable.md]
- **timelockController_VAULT_MANAGER_ROLE** (`contract TimelockControllerEnumerable`) [lib/openzeppelin-community-contracts/contracts/governance/TimelockControllerEnumerable.sol/contract_TimelockControllerEnumerable.md]
- **guardians** (`address[]`)
- **strategists** (`address[]`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: ConfigureAuthRolesScript.run() (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
```
