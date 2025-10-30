# Function: run()

**Contract**: [script/CryticAaveStrategyVaultMock.s.sol/contract_CryticAaveStrategyVaultMockScript.md]

## Metadata

- **Contract**: CryticAaveStrategyVaultMockScript
- **Signature**: `run()`
- **Visibility**: public
- **Source Range**: 1240:196:129

## Implementation

```solidity
function run() public {
    vm.startBroadcast();
    console.log("CryticAaveStrategyVaultMock", address(deploy(auth, asset, firstDepositAmount, pool)));
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

### deploy(contract Auth,contract IERC20Metadata,uint256,contract PoolMock)

- **Kind**: internal
- **Source**: 1442:1248:129
- **Link**: `script/CryticAaveStrategyVaultMock.s.sol:CryticAaveStrategyVaultMockScript:deploy(contract Auth,contract IERC20Metadata,uint256,contract PoolMock)`

```solidity
function deploy(Auth auth_, IERC20Metadata asset_, uint256 firstDepositAmount_, PoolMock pool_) public returns (CryticAaveStrategyVaultMock cryticAaveStrategyVaultMock) {
    string memory name = string.concat("Very Liquid Crytic Aave ", asset_.name(), " Strategy Mock Vault");
    string memory symbol = string.concat("vlv", "Aave", asset_.symbol(), "Mock");
    address implementation = address(new CryticAaveStrategyVaultMock());
    bytes memory initializationData = abi.encodeCall(AaveStrategyVault.initialize, (auth_, asset_, name, symbol, fundingAccount, firstDepositAmount_, pool_));
    bytes memory creationCode = abi.encodePacked(type(ERC1967Proxy).creationCode, abi.encode(implementation, initializationData));
    bytes32 salt = keccak256(initializationData);
    cryticAaveStrategyVaultMock = CryticAaveStrategyVaultMock(create2Deployer.computeAddress(salt, keccak256(creationCode)));
    asset_.forceApprove(address(cryticAaveStrategyVaultMock), firstDepositAmount_);
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
- **pool** (`contract PoolMock`)
- **fundingAccount** (`address`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: CryticAaveStrategyVaultMockScript.run() (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
  └─ [1] ⚙️ FUNCTION: console.log(string,address) (NodeID: 1)
      💬 Args: ["CryticAaveStrategyVaultMock", address(deploy(auth, asset, firstDepositAmount, pool))]
      👁️  Def: internal
    ├─ [2] ⚙️ FUNCTION: CryticAaveStrategyVaultMockScript.deploy(contract Auth,contract IERC20Metadata,uint256,contract PoolMock) (NodeID: 4)
    │   💬 Args: [auth, asset, firstDepositAmount, pool]
    │   👁️  Def: public
    └─ [2] ⚙️ FUNCTION: StdUtils._sendLogPayload(bytes) (NodeID: 2)
        💬 Args: [abi.encodeWithSignature("log(string,address)", p0, p1)]
        👁️  Def: internal
      └─ [3] ⚙️ FUNCTION: StdUtils._castLogPayloadViewToPure(function (bytes) view) (NodeID: 3)
          💬 Args: [_sendLogPayloadView]
          👁️  Def: internal
```
