# Function: run()

**Contract**: [script/CryticERC4626StrategyVaultMock.s.sol/contract_CryticERC4626StrategyVaultMockScript.md]

## Metadata

- **Contract**: CryticERC4626StrategyVaultMockScript
- **Signature**: `run()`
- **Visibility**: public
- **Source Range**: 1238:193:131

## Implementation

```solidity
function run() public {
    vm.startBroadcast();
    console.log("CryticERC4626StrategyVaultMock", address(deploy(auth, firstDepositAmount, vault)));
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

### deploy(contract Auth,uint256,contract VaultMock)

- **Kind**: internal
- **Source**: 1437:1276:131
- **Link**: `script/CryticERC4626StrategyVaultMock.s.sol:CryticERC4626StrategyVaultMockScript:deploy(contract Auth,uint256,contract VaultMock)`

```solidity
function deploy(Auth auth_, uint256 firstDepositAmount_, VaultMock vault_) public returns (CryticERC4626StrategyVaultMock cryticERC4626StrategyVaultMock) {
    string memory name = string.concat("Very Liquid ", vault_.name(), " Strategy Mock Vault");
    string memory symbol = string.concat("vlv", vault_.symbol(), "Mock");
    address implementation = address(new CryticERC4626StrategyVaultMock());
    bytes memory initializationData = abi.encodeCall(ERC4626StrategyVault.initialize, (auth_, name, symbol, fundingAccount, firstDepositAmount_, vault_));
    bytes memory creationCode = abi.encodePacked(type(ERC1967Proxy).creationCode, abi.encode(implementation, initializationData));
    bytes32 salt = keccak256(initializationData);
    cryticERC4626StrategyVaultMock = CryticERC4626StrategyVaultMock(create2Deployer.computeAddress(salt, keccak256(creationCode)));
    IERC20Metadata(address(vault_.asset())).forceApprove(address(cryticERC4626StrategyVaultMock), firstDepositAmount_);
    create2Deployer.deploy(0, salt, abi.encodePacked(type(ERC1967Proxy).creationCode, abi.encode(implementation, initializationData)));
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
- **Vm::stopBroadcast()**

## State Variable Reads

- **auth** (`contract Auth`) [src/Auth.sol/contract_Auth.md]
- **firstDepositAmount** (`uint256`)
- **vault** (`contract VaultMock`)
- **fundingAccount** (`address`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: CryticERC4626StrategyVaultMockScript.run() (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
  └─ [1] ⚙️ FUNCTION: console.log(string,address) (NodeID: 1)
      💬 Args: ["CryticERC4626StrategyVaultMock", address(deploy(auth, firstDepositAmount, vault))]
      👁️  Def: internal
    ├─ [2] ⚙️ FUNCTION: CryticERC4626StrategyVaultMockScript.deploy(contract Auth,uint256,contract VaultMock) (NodeID: 4)
    │   💬 Args: [auth, firstDepositAmount, vault]
    │   👁️  Def: public
    └─ [2] ⚙️ FUNCTION: StdUtils._sendLogPayload(bytes) (NodeID: 2)
        💬 Args: [abi.encodeWithSignature("log(string,address)", p0, p1)]
        👁️  Def: internal
      └─ [3] ⚙️ FUNCTION: StdUtils._castLogPayloadViewToPure(function (bytes) view) (NodeID: 3)
          💬 Args: [_sendLogPayloadView]
          👁️  Def: internal
```
