# Function: constructor(uint256,address[],address[],address)

**Contract**: [lib/openzeppelin-community-contracts/contracts/governance/TimelockControllerEnumerable.sol/contract_TimelockControllerEnumerable.md]

## Metadata

- **Contract**: TimelockControllerEnumerable
- **Signature**: `constructor(uint256,address[],address[],address)`
- **Visibility**: public
- **Source Range**: 1764:199:53

## Implementation

```solidity
constructor(uint256 minDelay, address[] memory proposers, address[] memory executors, address admin) TimelockController(minDelay,proposers,executors,admin) {}
```

## Related Implementations

### (uint256,address[],address[],address)

- **Kind**: internal
- **Source**: 4248:761:72
- **Link**: `lib/openzeppelin-contracts/contracts/governance/TimelockController.sol:TimelockController:constructor(uint256,address[],address[],address)`

```solidity
///  @dev Initializes the contract with the following parameters:
///  - `minDelay`: initial minimum delay in seconds for operations
///  - `proposers`: accounts to be granted proposer and canceller roles
///  - `executors`: accounts to be granted executor role
///  - `admin`: optional account to be granted admin role; disable with zero address
///  IMPORTANT: The optional admin can aid with initial configuration of roles after deployment
///  without being subject to delay, but this role should be subsequently renounced in favor of
///  administration through timelocked proposals. Previous versions of this contract would assign
///  this admin to the deployer automatically and should be renounced as well.
constructor(uint256 minDelay, address[] memory proposers, address[] memory executors, address admin) {
    _grantRole(DEFAULT_ADMIN_ROLE, address(this));
    if (admin != address(0)) {
        _grantRole(DEFAULT_ADMIN_ROLE, admin);
    }
    for (uint256 i = 0; i < proposers.length; ++i) {
        _grantRole(PROPOSER_ROLE, proposers[i]);
        _grantRole(CANCELLER_ROLE, proposers[i]);
    }
    for (uint256 i = 0; i < executors.length; ++i) {
        _grantRole(EXECUTOR_ROLE, executors[i]);
    }
    _minDelay = minDelay;
    emit MinDelayChange(0, minDelay);
}
```

### _grantRole(bytes32,address)

- **Kind**: internal
- **Source**: 6179:316:68
- **Link**: `lib/openzeppelin-contracts/contracts/access/AccessControl.sol:AccessControl:_grantRole(bytes32,address)`

```solidity
///  @dev Attempts to grant `role` to `account` and returns a boolean indicating if `role` was granted.
///  Internal function without access restriction.
///  May emit a {RoleGranted} event.
function _grantRole(bytes32 role, address account) virtual internal returns (bool) {
    if (!hasRole(role, account)) {
        _roles[role].hasRole[account] = true;
        emit RoleGranted(role, account, _msgSender());
        return true;
    } else {
        return false;
    }
}
```

### hasRole(bytes32,address)

- **Kind**: internal
- **Source**: 2854:136:68
- **Link**: `lib/openzeppelin-contracts/contracts/access/AccessControl.sol:AccessControl:hasRole(bytes32,address)`

```solidity
///  @dev Returns `true` if `account` has been granted `role`.
function hasRole(bytes32 role, address account) virtual public view returns (bool) {
    return _roles[role].hasRole[account];
}
```

### _msgSender()

- **Kind**: internal
- **Source**: 656:96:98
- **Link**: `lib/openzeppelin-contracts/contracts/utils/Context.sol:Context:_msgSender()`

```solidity
function _msgSender() virtual internal view returns (address) {
    return msg.sender;
}
```

## State Variable Reads

- **PROPOSER_ROLE** (`bytes32`)
- **CANCELLER_ROLE** (`bytes32`)
- **EXECUTOR_ROLE** (`bytes32`)
- **_roles** (`mapping(bytes32 => struct AccessControl.RoleData)`)

## State Variable Writes

- **_minDelay** (`uint256`)
- **_roles** (`mapping(bytes32 => struct AccessControl.RoleData)`)

## Call Tree

```
┌─ [0] 🏗️ CONSTRUCTOR: TimelockControllerEnumerable.constructor(uint256,address[],address[],address) (NodeID: 0)
    💬 Args: [no args]
    🏗️  Contract: TimelockControllerEnumerable
  └─ [1] 🏗️ CONSTRUCTOR: TimelockController.constructor(uint256,address[],address[],address) (NodeID: 1)
      💬 Args: [minDelay, proposers, executors, admin]
      🏗️  Contract: TimelockController
    ├─ [2] ⚙️ FUNCTION: AccessControl._grantRole(bytes32,address) (NodeID: 2)
    │   💬 Args: [DEFAULT_ADMIN_ROLE, address(this)]
    │   👁️  Def: internal
    │ ├─ [3] ⚙️ FUNCTION: AccessControl.hasRole(bytes32,address) (NodeID: 3)
    │ │   💬 Args: [role, account]
    │ │   👁️  Def: public
    │ └─ [3] ⚙️ FUNCTION: Context._msgSender() (NodeID: 4)
    │     💬 Args: [no args]
    │     👁️  Def: internal
    ├─ [2] ⚙️ FUNCTION: AccessControl._grantRole(bytes32,address) (NodeID: 5)
    │   💬 Args: [DEFAULT_ADMIN_ROLE, admin]
    │   👁️  Def: internal
    │ ├─ [3] ⚙️ FUNCTION: AccessControl.hasRole(bytes32,address) (NodeID: 6)
    │ │   💬 Args: [role, account]
    │ │   👁️  Def: public
    │ └─ [3] ⚙️ FUNCTION: Context._msgSender() (NodeID: 7)
    │     💬 Args: [no args]
    │     👁️  Def: internal
    ├─ [2] ⚙️ FUNCTION: AccessControl._grantRole(bytes32,address) (NodeID: 8)
    │   💬 Args: [PROPOSER_ROLE, proposers[i]]
    │   👁️  Def: internal
    │ ├─ [3] ⚙️ FUNCTION: AccessControl.hasRole(bytes32,address) (NodeID: 9)
    │ │   💬 Args: [role, account]
    │ │   👁️  Def: public
    │ └─ [3] ⚙️ FUNCTION: Context._msgSender() (NodeID: 10)
    │     💬 Args: [no args]
    │     👁️  Def: internal
    ├─ [2] ⚙️ FUNCTION: AccessControl._grantRole(bytes32,address) (NodeID: 11)
    │   💬 Args: [CANCELLER_ROLE, proposers[i]]
    │   👁️  Def: internal
    │ ├─ [3] ⚙️ FUNCTION: AccessControl.hasRole(bytes32,address) (NodeID: 12)
    │ │   💬 Args: [role, account]
    │ │   👁️  Def: public
    │ └─ [3] ⚙️ FUNCTION: Context._msgSender() (NodeID: 13)
    │     💬 Args: [no args]
    │     👁️  Def: internal
    └─ [2] ⚙️ FUNCTION: AccessControl._grantRole(bytes32,address) (NodeID: 14)
        💬 Args: [EXECUTOR_ROLE, executors[i]]
        👁️  Def: internal
      ├─ [3] ⚙️ FUNCTION: AccessControl.hasRole(bytes32,address) (NodeID: 15)
      │   💬 Args: [role, account]
      │   👁️  Def: public
      └─ [3] ⚙️ FUNCTION: Context._msgSender() (NodeID: 16)
          💬 Args: [no args]
          👁️  Def: internal
```
