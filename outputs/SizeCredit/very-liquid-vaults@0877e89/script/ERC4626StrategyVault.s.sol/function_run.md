# Function: run()

**Contract**: [script/ERC4626StrategyVault.s.sol/contract_ERC4626StrategyVaultScript.md]

## Metadata

- **Contract**: ERC4626StrategyVaultScript
- **Signature**: `run()`
- **Visibility**: public
- **Source Range**: 1089:183:133

## Implementation

```solidity
function run() public {
    vm.startBroadcast();
    console.log("ERC4626StrategyVault", address(deploy(auth, firstDepositAmount, vault)));
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

### deploy(contract Auth,uint256,contract IERC4626)

- **Kind**: internal
- **Source**: 1278:1168:133
- **Link**: `script/ERC4626StrategyVault.s.sol:ERC4626StrategyVaultScript:deploy(contract Auth,uint256,contract IERC4626)`

```solidity
function deploy(Auth auth_, uint256 firstDepositAmount_, IERC4626 vault_) public returns (ERC4626StrategyVault erc4626StrategyVault) {
    string memory name = string.concat("Very Liquid ", vault_.name(), " Strategy Vault");
    string memory symbol = string.concat("vlv", vault_.symbol());
    address implementation = address(new ERC4626StrategyVault());
    bytes memory initializationData = abi.encodeCall(ERC4626StrategyVault.initialize, (auth_, name, symbol, fundingAccount, firstDepositAmount_, vault_));
    bytes memory creationCode = abi.encodePacked(type(ERC1967Proxy).creationCode, abi.encode(implementation, initializationData));
    bytes32 salt = keccak256(initializationData);
    erc4626StrategyVault = ERC4626StrategyVault(create2Deployer.computeAddress(salt, keccak256(creationCode)));
    IERC20Metadata(address(vault_.asset())).forceApprove(address(erc4626StrategyVault), firstDepositAmount_);
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
- **vault** (`contract IERC4626`) [lib/openzeppelin-contracts/contracts/interfaces/IERC4626.sol/interface_IERC4626.md]
- **fundingAccount** (`address`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: ERC4626StrategyVaultScript.run() (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
  └─ [1] ⚙️ FUNCTION: console.log(string,address) (NodeID: 1)
      💬 Args: ["ERC4626StrategyVault", address(deploy(auth, firstDepositAmount, vault))]
      👁️  Def: internal
    ├─ [2] ⚙️ FUNCTION: ERC4626StrategyVaultScript.deploy(contract Auth,uint256,contract IERC4626) (NodeID: 4)
    │   💬 Args: [auth, firstDepositAmount, vault]
    │   👁️  Def: public
    └─ [2] ⚙️ FUNCTION: StdUtils._sendLogPayload(bytes) (NodeID: 2)
        💬 Args: [abi.encodeWithSignature("log(string,address)", p0, p1)]
        👁️  Def: internal
      └─ [3] ⚙️ FUNCTION: StdUtils._castLogPayloadViewToPure(function (bytes) view) (NodeID: 3)
          💬 Args: [_sendLogPayloadView]
          👁️  Def: internal
```
