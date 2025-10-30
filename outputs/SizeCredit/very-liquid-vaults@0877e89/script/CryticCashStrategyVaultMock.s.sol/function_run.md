# Function: run()

**Contract**: [script/CryticCashStrategyVaultMock.s.sol/contract_CryticCashStrategyVaultMockScript.md]

## Metadata

- **Contract**: CryticCashStrategyVaultMockScript
- **Signature**: `run()`
- **Visibility**: public
- **Source Range**: 1099:190:130

## Implementation

```solidity
function run() public {
    vm.startBroadcast();
    console.log("CryticCashStrategyVaultMock", address(deploy(auth, asset, firstDepositAmount)));
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

### deploy(contract Auth,contract IERC20Metadata,uint256)

- **Kind**: internal
- **Source**: 1295:1207:130
- **Link**: `script/CryticCashStrategyVaultMock.s.sol:CryticCashStrategyVaultMockScript:deploy(contract Auth,contract IERC20Metadata,uint256)`

```solidity
function deploy(Auth auth_, IERC20Metadata asset_, uint256 firstDepositAmount_) public returns (CryticCashStrategyVaultMock cryticCashStrategyVaultMock) {
    string memory name = string.concat("Very Liquid Crytic Cash ", asset_.name(), " Strategy Mock Vault");
    string memory symbol = string.concat("vlv", "Cash", asset_.symbol(), "Mock");
    address implementation = address(new CryticCashStrategyVaultMock());
    bytes memory initializationData = abi.encodeCall(BaseVault.initialize, (auth_, asset_, name, symbol, fundingAccount, firstDepositAmount_));
    bytes memory creationCode = abi.encodePacked(type(ERC1967Proxy).creationCode, abi.encode(implementation, initializationData));
    bytes32 salt = keccak256(initializationData);
    cryticCashStrategyVaultMock = CryticCashStrategyVaultMock(create2Deployer.computeAddress(salt, keccak256(creationCode)));
    asset_.forceApprove(address(cryticCashStrategyVaultMock), firstDepositAmount_);
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
- **asset** (`contract IERC20Metadata`) [lib/openzeppelin-contracts/contracts/token/ERC20/extensions/IERC20Metadata.sol/interface_IERC20Metadata.md]
- **firstDepositAmount** (`uint256`)
- **fundingAccount** (`address`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: CryticCashStrategyVaultMockScript.run() (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
  └─ [1] ⚙️ FUNCTION: console.log(string,address) (NodeID: 1)
      💬 Args: ["CryticCashStrategyVaultMock", address(deploy(auth, asset, firstDepositAmount))]
      👁️  Def: internal
    ├─ [2] ⚙️ FUNCTION: CryticCashStrategyVaultMockScript.deploy(contract Auth,contract IERC20Metadata,uint256) (NodeID: 4)
    │   💬 Args: [auth, asset, firstDepositAmount]
    │   👁️  Def: public
    └─ [2] ⚙️ FUNCTION: StdUtils._sendLogPayload(bytes) (NodeID: 2)
        💬 Args: [abi.encodeWithSignature("log(string,address)", p0, p1)]
        👁️  Def: internal
      └─ [3] ⚙️ FUNCTION: StdUtils._castLogPayloadViewToPure(function (bytes) view) (NodeID: 3)
          💬 Args: [_sendLogPayloadView]
          👁️  Def: internal
```
