# Function: run()

**Contract**: [script/CryticVeryLiquidVaultMock.s.sol/contract_CryticVeryLiquidVaultMockScript.md]

## Metadata

- **Contract**: CryticVeryLiquidVaultMockScript
- **Signature**: `run()`
- **Visibility**: public
- **Source Range**: 1414:200:132

## Implementation

```solidity
function run() public {
    vm.startBroadcast();
    console.log("CryticVeryLiquidVaultMock", address(deploy(auth, asset, firstDepositAmount, strategies)));
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

### deploy(contract Auth,contract IERC20Metadata,uint256,contract IVault[])

- **Kind**: internal
- **Source**: 1620:1236:132
- **Link**: `script/CryticVeryLiquidVaultMock.s.sol:CryticVeryLiquidVaultMockScript:deploy(contract Auth,contract IERC20Metadata,uint256,contract IVault[])`

```solidity
function deploy(Auth auth_, IERC20Metadata asset_, uint256 firstDepositAmount_, IVault[] memory strategies_) public returns (CryticVeryLiquidVaultMock cryticVeryLiquidVaultMock) {
    string memory name = string.concat("Very Liquid Crytic ", asset_.name(), " Vault");
    string memory symbol = string.concat("vlv", "Crytic", asset_.symbol(), "Mock");
    address implementation = address(new CryticVeryLiquidVaultMock());
    bytes memory initializationData = abi.encodeCall(VeryLiquidVault.initialize, (auth_, asset_, name, symbol, fundingAccount, firstDepositAmount_, strategies_));
    bytes memory creationCode = abi.encodePacked(type(ERC1967Proxy).creationCode, abi.encode(implementation, initializationData));
    bytes32 salt = keccak256(initializationData);
    cryticVeryLiquidVaultMock = CryticVeryLiquidVaultMock(create2Deployer.computeAddress(salt, keccak256(creationCode)));
    asset_.forceApprove(address(cryticVeryLiquidVaultMock), firstDepositAmount_);
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
- **strategies** (`contract IVault[]`) [src/IVault.sol/interface_IVault.md]
- **fundingAccount** (`address`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: CryticVeryLiquidVaultMockScript.run() (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
  └─ [1] ⚙️ FUNCTION: console.log(string,address) (NodeID: 1)
      💬 Args: ["CryticVeryLiquidVaultMock", address(deploy(auth, asset, firstDepositAmount, strategies))]
      👁️  Def: internal
    ├─ [2] ⚙️ FUNCTION: CryticVeryLiquidVaultMockScript.deploy(contract Auth,contract IERC20Metadata,uint256,contract IVault[]) (NodeID: 4)
    │   💬 Args: [auth, asset, firstDepositAmount, strategies]
    │   👁️  Def: public
    └─ [2] ⚙️ FUNCTION: StdUtils._sendLogPayload(bytes) (NodeID: 2)
        💬 Args: [abi.encodeWithSignature("log(string,address)", p0, p1)]
        👁️  Def: internal
      └─ [3] ⚙️ FUNCTION: StdUtils._castLogPayloadViewToPure(function (bytes) view) (NodeID: 3)
          💬 Args: [_sendLogPayloadView]
          👁️  Def: internal
```
