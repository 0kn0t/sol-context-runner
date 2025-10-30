# Function: run()

**Contract**: [script/TimelockControllerEnumerables.s.sol/contract_TimelockControllerEnumerablesScript.md]

## Metadata

- **Contract**: TimelockControllerEnumerablesScript
- **Signature**: `run()`
- **Visibility**: public
- **Source Range**: 751:1576:139

## Implementation

```solidity
function run() public {
    vm.startBroadcast();
    address[] memory adminTimelockAddresses = new address[](1);
    adminTimelockAddresses[0] = adminMultisig;
    TimelockControllerEnumerable timelockController_DEFAULT_ADMIN_ROLE = new TimelockControllerEnumerable(7 days, adminTimelockAddresses, adminTimelockAddresses, msg.sender);
    console.log("TimelockController (DEFAULT_ADMIN_ROLE)", address(timelockController_DEFAULT_ADMIN_ROLE));
    for (uint256 i = 0; i < guardians.length; i++) {
        timelockController_DEFAULT_ADMIN_ROLE.grantRole(timelockController_DEFAULT_ADMIN_ROLE.CANCELLER_ROLE(), guardians[i]);
    }
    timelockController_DEFAULT_ADMIN_ROLE.renounceRole(DEFAULT_ADMIN_ROLE, msg.sender);
    address[] memory vaultManagerAddresses = new address[](1);
    vaultManagerAddresses[0] = vaultManagerMultisig;
    TimelockControllerEnumerable timelockController_VAULT_MANAGER_ROLE = new TimelockControllerEnumerable(1 days, vaultManagerAddresses, vaultManagerAddresses, msg.sender);
    console.log("TimelockController (VAULT_MANAGER_ROLE)", address(timelockController_VAULT_MANAGER_ROLE));
    for (uint256 i = 0; i < guardians.length; i++) {
        timelockController_VAULT_MANAGER_ROLE.grantRole(timelockController_VAULT_MANAGER_ROLE.CANCELLER_ROLE(), guardians[i]);
    }
    timelockController_VAULT_MANAGER_ROLE.renounceRole(DEFAULT_ADMIN_ROLE, msg.sender);
    vm.stopBroadcast();
}
```

## Related Implementations

### log(string,address)

- **Kind**: internal
- **Source**: 7740:145:49
- **Link**: `lib/forge-std/src/console.sol:console:log(string,address)`

```solidity
function log(string memory p0, address p1) internal pure {
    _sendLogPayload(abi.encodeWithSignature("log(string,address)", p0, p1));
}
```

### _sendLogPayload(bytes)

- **Kind**: internal
- **Source**: 9016:133:47
- **Link**: `lib/forge-std/src/StdUtils.sol:StdUtils:_sendLogPayload(bytes)`

```solidity
function _sendLogPayload(bytes memory payload) internal pure {
    _castLogPayloadViewToPure(_sendLogPayloadView)(payload);
}
```

### _castLogPayloadViewToPure(function (bytes)

- **Kind**: internal
- **Source**: 8775:235:47
- **Link**: `lib/forge-std/src/StdUtils.sol:StdUtils:_castLogPayloadViewToPure(function (bytes) view)`

```solidity
function _castLogPayloadViewToPure(function(bytes memory) internal view fnIn) internal pure returns (function(bytes memory) internal pure fnOut) {
    assembly {
        fnOut := fnIn
    }
}
```

## External Calls

- **Vm::startBroadcast()**
- **TimelockControllerEnumerable::grantRole(bytes32,address)**
- **TimelockControllerEnumerable::CANCELLER_ROLE()**
- **TimelockControllerEnumerable::renounceRole(bytes32,address)**
- **Vm::stopBroadcast()**

## State Variable Reads

- **adminMultisig** (`address`)
- **guardians** (`address[]`)
- **vaultManagerMultisig** (`address`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: TimelockControllerEnumerablesScript.run() (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
  ├─ [1] ⚙️ FUNCTION: console.log(string,address) (NodeID: 1)
  │   💬 Args: ["TimelockController (DEFAULT_ADMIN_ROLE)", address(timelockController_DEFAULT_ADMIN_ROLE)]
  │   👁️  Def: internal
  │ └─ [2] ⚙️ FUNCTION: StdUtils._sendLogPayload(bytes) (NodeID: 2)
  │     💬 Args: [abi.encodeWithSignature("log(string,address)", p0, p1)]
  │     👁️  Def: internal
  │   └─ [3] ⚙️ FUNCTION: StdUtils._castLogPayloadViewToPure(function (bytes) view) (NodeID: 3)
  │       💬 Args: [_sendLogPayloadView]
  │       👁️  Def: internal
  └─ [1] ⚙️ FUNCTION: console.log(string,address) (NodeID: 4)
      💬 Args: ["TimelockController (VAULT_MANAGER_ROLE)", address(timelockController_VAULT_MANAGER_ROLE)]
      👁️  Def: internal
    └─ [2] ⚙️ FUNCTION: StdUtils._sendLogPayload(bytes) (NodeID: 5)
        💬 Args: [abi.encodeWithSignature("log(string,address)", p0, p1)]
        👁️  Def: internal
      └─ [3] ⚙️ FUNCTION: StdUtils._castLogPayloadViewToPure(function (bytes) view) (NodeID: 6)
          💬 Args: [_sendLogPayloadView]
          👁️  Def: internal
```
