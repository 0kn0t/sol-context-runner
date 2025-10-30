# Function: run()

**Contract**: [script/ProposeSafeTxUpgradeVeryLiquidVaults.s.sol/contract_ProposeSafeTxUpgradeVeryLiquidVaultsScript.md]

## Metadata

- **Contract**: ProposeSafeTxUpgradeVeryLiquidVaultsScript
- **Signature**: `run()`
- **Visibility**: public
- **Source Range**: 945:1491:138

## Implementation

```solidity
function run() public {
    vm.startBroadcast();
    address newImplementation = address(new VeryLiquidVault());
    console.log("New implementation", address(newImplementation));
    address[] memory veryLiquidVaults = (block.chainid == 1) ? getMainnetVeryLiquidVaults() : ((block.chainid == 8453) ? getBaseVeryLiquidVaults() : new address[](0));
    TimelockController timelockController = TimelockController(payable(addresses[block.chainid][Contract.TimelockController_DEFAULT_ADMIN_ROLE]));
    address[] memory multisigTargets = new address[](veryLiquidVaults.length);
    bytes[] memory multisigDatas = new bytes[](veryLiquidVaults.length);
    for (uint256 i = 0; i < veryLiquidVaults.length; i++) {
        address target = veryLiquidVaults[i];
        uint256 value = 0;
        bytes memory data = abi.encodeCall(UUPSUpgradeable.upgradeToAndCall, (newImplementation, ""));
        bytes32 predecessor = bytes32(0);
        bytes32 salt = keccak256(abi.encode(target, value, data, predecessor));
        uint256 delay = timelockController.getMinDelay();
        multisigTargets[i] = address(timelockController);
        multisigDatas[i] = abi.encodeCall(timelockController.schedule, (target, value, data, predecessor, salt, delay));
    }
    safe.proposeTransactions(multisigTargets, multisigDatas, signer, derivationPath);
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

### getMainnetVeryLiquidVaults()

- **Kind**: internal
- **Source**: 2442:268:138
- **Link**: `script/ProposeSafeTxUpgradeVeryLiquidVaults.s.sol:ProposeSafeTxUpgradeVeryLiquidVaultsScript:getMainnetVeryLiquidVaults()`

```solidity
function getMainnetVeryLiquidVaults() public view returns (address[] memory ans) {
    ans = new address[](2);
    ans[0] = addresses[1][Contract.VeryLiquidVault_Core];
    ans[1] = addresses[1][Contract.VeryLiquidVault_Frontier];
    return ans;
}
```

### getBaseVeryLiquidVaults()

- **Kind**: internal
- **Source**: 2716:202:138
- **Link**: `script/ProposeSafeTxUpgradeVeryLiquidVaults.s.sol:ProposeSafeTxUpgradeVeryLiquidVaultsScript:getBaseVeryLiquidVaults()`

```solidity
function getBaseVeryLiquidVaults() public view returns (address[] memory ans) {
    ans = new address[](1);
    ans[0] = addresses[8453][Contract.VeryLiquidVault_Core];
    return ans;
}
```

### proposeTransactions(struct Safe.Client,address[],bytes[],address,string)

- **Kind**: internal
- **Source**: 11539:804:121
- **Link**: `lib/safe-utils/src/Safe.sol:Safe:proposeTransactions(struct Safe.Client,address[],bytes[],address,string)`

```solidity
function proposeTransactions(Client storage self, address[] memory targets, bytes[] memory datas, address sender, string memory derivationPath) internal returns (bytes32) {
    (address to, bytes memory data) = getProposeTransactionsTargetAndData(self, targets, datas);
    ExecTransactionParams memory params = ExecTransactionParams({to: to, value: 0, data: data, operation: Enum.Operation.DelegateCall, sender: sender, signature: sign(self, to, data, Enum.Operation.DelegateCall, sender, derivationPath), nonce: getNonce(self)});
    return proposeTransaction(self, params);
}
```

### getProposeTransactionsTargetAndData(struct Safe.Client,address[],bytes[])

- **Kind**: internal
- **Source**: 10637:896:121
- **Link**: `lib/safe-utils/src/Safe.sol:Safe:getProposeTransactionsTargetAndData(struct Safe.Client,address[],bytes[])`

```solidity
function getProposeTransactionsTargetAndData(Client storage self, address[] memory targets, bytes[] memory datas) internal view returns (address, bytes memory) {
    if (targets.length != datas.length) {
        revert ArrayLengthsMismatch(targets.length, datas.length);
    }
    bytes1 operation = bytes1(uint8(Enum.Operation.Call));
    uint256 value = 0;
    bytes memory transactions;
    for (uint256 i = 0; i < targets.length; i++) {
        uint256 dataLength = datas[i].length;
        transactions = abi.encodePacked(transactions, abi.encodePacked(operation, targets[i], value, dataLength, datas[i]));
    }
    address to = address(getMultiSendCallOnly(self, block.chainid));
    bytes memory data = abi.encodeCall(MultiSendCallOnly.multiSend, (transactions));
    return (to, data);
}
```

### getMultiSendCallOnly(struct Safe.Client,uint256)

- **Kind**: internal
- **Source**: 6497:361:121
- **Link**: `lib/safe-utils/src/Safe.sol:Safe:getMultiSendCallOnly(struct Safe.Client,uint256)`

```solidity
function getMultiSendCallOnly(Client storage self, uint256 chainId) internal view returns (MultiSendCallOnly) {
    MultiSendCallOnly multiSendCallOnly = instance(self).multiSendCallOnly[chainId];
    if (address(multiSendCallOnly) == address(0)) {
        revert MultiSendCallOnlyNotFound(chainId);
    }
    return multiSendCallOnly;
}
```

### instance(struct Safe.Client)

- **Kind**: internal
- **Source**: 6062:145:121
- **Link**: `lib/safe-utils/src/Safe.sol:Safe:instance(struct Safe.Client)`

```solidity
function instance(Client storage self) internal view returns (Instance storage) {
    return self.instances[self.instances.length - 1];
}
```

### sign(struct Safe.Client,address,bytes,enum Enum.Operation,address,string)

- **Kind**: internal
- **Source**: 14992:2050:121
- **Link**: `lib/safe-utils/src/Safe.sol:Safe:sign(struct Safe.Client,address,bytes,enum Enum.Operation,address,string)`

```solidity
function sign(Client storage self, address to, bytes memory data, Enum.Operation operation, address sender, string memory derivationPath) internal returns (bytes memory) {
    uint256 nonce = getNonce(self);
    if (bytes(derivationPath).length > 0) {
        string[] memory inputs = new string[](8);
        inputs[0] = "cast";
        inputs[1] = "wallet";
        inputs[2] = "sign";
        inputs[3] = "--ledger";
        inputs[4] = "--mnemonic-derivation-path";
        inputs[5] = derivationPath;
        inputs[6] = "--data";
        inputs[7] = string.concat("{\"domain\":{\"chainId\":\"", vm.toString(block.chainid), "\",\"verifyingContract\":\"", vm.toString(instance(self).safe), "\"},\"message\":{\"to\":\"", vm.toString(to), "\",\"value\":\"0\",\"data\":\"", vm.toString(data), "\",\"operation\":", vm.toString(uint8(operation)), ",\"baseGas\":\"0\",\"gasPrice\":\"0\",\"gasToken\":\"0x0000000000000000000000000000000000000000\",\"refundReceiver\":\"0x0000000000000000000000000000000000000000\",\"nonce\":", vm.toString(nonce), ",\"safeTxGas\":\"0\"},\"primaryType\":\"SafeTx\",\"types\":{\"SafeTx\":[{\"name\":\"to\",\"type\":\"address\"},{\"name\":\"value\",\"type\":\"uint256\"},{\"name\":\"data\",\"type\":\"bytes\"},{\"name\":\"operation\",\"type\":\"uint8\"},{\"name\":\"safeTxGas\",\"type\":\"uint256\"},{\"name\":\"baseGas\",\"type\":\"uint256\"},{\"name\":\"gasPrice\",\"type\":\"uint256\"},{\"name\":\"gasToken\",\"type\":\"address\"},{\"name\":\"refundReceiver\",\"type\":\"address\"},{\"name\":\"nonce\",\"type\":\"uint256\"}]}}");
        bytes memory output = vm.ffi(inputs);
        return output;
    } else {
        Signature memory sig;
        (sig.v, sig.r, sig.s) = vm.sign(sender, getSafeTxHash(self, to, 0, data, operation, nonce));
        return abi.encodePacked(sig.r, sig.s, sig.v);
    }
}
```

### getNonce(struct Safe.Client)

- **Kind**: internal
- **Source**: 6864:141:121
- **Link**: `lib/safe-utils/src/Safe.sol:Safe:getNonce(struct Safe.Client)`

```solidity
function getNonce(Client storage self) internal view returns (uint256) {
    return ISafeSmartAccount(instance(self).safe).nonce();
}
```

### getSafeTxHash(struct Safe.Client,address,uint256,bytes,enum Enum.Operation,uint256)

- **Kind**: internal
- **Source**: 7011:388:121
- **Link**: `lib/safe-utils/src/Safe.sol:Safe:getSafeTxHash(struct Safe.Client,address,uint256,bytes,enum Enum.Operation,uint256)`

```solidity
function getSafeTxHash(Client storage self, address to, uint256 value, bytes memory data, Enum.Operation operation, uint256 nonce) internal view returns (bytes32) {
    return ISafeSmartAccount(instance(self).safe).getTransactionHash(to, value, data, operation, 0, 0, 0, address(0), address(0), nonce);
}
```

### proposeTransaction(struct Safe.Client,struct Safe.ExecTransactionParams)

- **Kind**: internal
- **Source**: 7506:1975:121
- **Link**: `lib/safe-utils/src/Safe.sol:Safe:proposeTransaction(struct Safe.Client,struct Safe.ExecTransactionParams)`

```solidity
function proposeTransaction(Client storage self, ExecTransactionParams memory params) internal returns (bytes32) {
    bytes32 safeTxHash = getSafeTxHash(self, params.to, params.value, params.data, params.operation, params.nonce);
    instance(self).requestBody = vm.serializeAddress(".proposeTransaction", "to", params.to);
    instance(self).requestBody = vm.serializeUint(".proposeTransaction", "value", params.value);
    instance(self).requestBody = vm.serializeBytes(".proposeTransaction", "data", params.data);
    instance(self).requestBody = vm.serializeUint(".proposeTransaction", "operation", uint8(params.operation));
    instance(self).requestBody = vm.serializeBytes32(".proposeTransaction", "contractTransactionHash", safeTxHash);
    instance(self).requestBody = vm.serializeAddress(".proposeTransaction", "sender", params.sender);
    instance(self).requestBody = vm.serializeBytes(".proposeTransaction", "signature", params.signature);
    instance(self).requestBody = vm.serializeUint(".proposeTransaction", "safeTxGas", 0);
    instance(self).requestBody = vm.serializeUint(".proposeTransaction", "baseGas", 0);
    instance(self).requestBody = vm.serializeUint(".proposeTransaction", "gasPrice", 0);
    instance(self).requestBody = vm.serializeUint(".proposeTransaction", "nonce", params.nonce);
    HTTP.Response memory response = instance(self).http.instance().POST(string.concat(getApiKitUrl(self, block.chainid), "/v1/safes/", vm.toString(instance(self).safe), "/multisig-transactions/")).withBody(instance(self).requestBody).request();
    if ((response.status < 200) || (response.status >= 300)) {
        revert ProposeTransactionFailed(response.status, response.data);
    }
    return safeTxHash;
}
```

### instance(struct HTTP.Client)

- **Kind**: internal
- **Source**: 1129:158:117
- **Link**: `lib/safe-utils/lib/solidity-http/src/HTTP.sol:HTTP:instance(struct HTTP.Client)`

```solidity
function instance(HTTP.Client storage client) internal view returns (HTTP.Request storage) {
    return client.requests[client.requests.length - 1];
}
```

### POST(struct HTTP.Request,string)

- **Kind**: internal
- **Source**: 2067:146:117
- **Link**: `lib/safe-utils/lib/solidity-http/src/HTTP.sol:HTTP:POST(struct HTTP.Request,string)`

```solidity
function POST(HTTP.Request storage req, string memory url) internal returns (HTTP.Request storage) {
    return POST(withUrl(req, url));
}
```

### getApiKitUrl(struct Safe.Client,uint256)

- **Kind**: internal
- **Source**: 6213:278:121
- **Link**: `lib/safe-utils/src/Safe.sol:Safe:getApiKitUrl(struct Safe.Client,uint256)`

```solidity
function getApiKitUrl(Client storage self, uint256 chainId) internal view returns (string memory) {
    string memory url = instance(self).urls[chainId];
    if (bytes(url).length == 0) {
        revert ApiKitUrlNotFound(chainId);
    }
    return url;
}
```

### POST(struct HTTP.Request)

- **Kind**: internal
- **Source**: 1924:137:117
- **Link**: `lib/safe-utils/lib/solidity-http/src/HTTP.sol:HTTP:POST(struct HTTP.Request)`

```solidity
function POST(HTTP.Request storage req) internal returns (HTTP.Request storage) {
    return withMethod(req, HTTP.Method.POST);
}
```

### withUrl(struct HTTP.Request,string)

- **Kind**: internal
- **Source**: 1293:152:117
- **Link**: `lib/safe-utils/lib/solidity-http/src/HTTP.sol:HTTP:withUrl(struct HTTP.Request,string)`

```solidity
function withUrl(HTTP.Request storage req, string memory url) internal returns (HTTP.Request storage) {
    req.url = url;
    return req;
}
```

### withMethod(struct HTTP.Request,enum HTTP.Method)

- **Kind**: internal
- **Source**: 1451:162:117
- **Link**: `lib/safe-utils/lib/solidity-http/src/HTTP.sol:HTTP:withMethod(struct HTTP.Request,enum HTTP.Method)`

```solidity
function withMethod(HTTP.Request storage req, HTTP.Method method) internal returns (HTTP.Request storage) {
    req.method = method;
    return req;
}
```

### withBody(struct HTTP.Request,string)

- **Kind**: internal
- **Source**: 3126:156:117
- **Link**: `lib/safe-utils/lib/solidity-http/src/HTTP.sol:HTTP:withBody(struct HTTP.Request,string)`

```solidity
function withBody(HTTP.Request storage req, string memory body) internal returns (HTTP.Request storage) {
    req.body = body;
    return req;
}
```

### request(struct HTTP.Request)

- **Kind**: internal
- **Source**: 4560:1246:117
- **Link**: `lib/safe-utils/lib/solidity-http/src/HTTP.sol:HTTP:request(struct HTTP.Request)`

```solidity
function request(Request storage req) internal returns (Response memory res) {
    string memory scriptStart = "response=$(curl -s -w \"\\n%{http_code}\" ";
    string memory scriptEnd = "); status=$(tail -n1 <<< \"$response\"); data=$(sed \"$ d\" <<< \"$response\");data=$(echo \"$data\" | tr -d \"\\n\"); cast abi-encode \"response(uint256,string)\" \"$status\" \"$data\";";
    string memory curlParams = "";
    for (uint256 i = 0; i < req.headers.length(); i++) {
        (string memory key, string memory value) = req.headers.at(i);
        curlParams = string.concat(curlParams, "-H \"", key, ": ", value, "\" ");
    }
    curlParams = string.concat(curlParams, " -X ", toString(req.method), " ");
    if (bytes(req.body).length > 0) {
        curlParams = string.concat(curlParams, "-d '", req.body, "' ");
    }
    string memory quotedURL = string.concat("\"", req.url, "\"");
    string[] memory inputs = new string[](3);
    inputs[0] = "bash";
    inputs[1] = "-c";
    inputs[2] = string.concat(scriptStart, curlParams, quotedURL, scriptEnd, "");
    bytes memory output = vm.ffi(inputs);
    (res.status, res.data) = abi.decode(output, (uint256, string));
}
```

### length(struct StringMap.StringToStringMap)

- **Kind**: internal
- **Source**: 1598:121:118
- **Link**: `lib/safe-utils/lib/solidity-http/src/StringMap.sol:StringMap:length(struct StringMap.StringToStringMap)`

```solidity
///  @dev Returns the number of key-value pairs in the map. O(1).
function length(StringToStringMap storage map) internal view returns (uint256) {
    return map._keys.length();
}
```

### length(struct StringSet.Set)

- **Kind**: internal
- **Source**: 2903:107:119
- **Link**: `lib/safe-utils/lib/solidity-http/src/StringSet.sol:StringSet:length(struct StringSet.Set)`

```solidity
///  @dev Returns the number of values on the set. O(1).
function length(Set storage set) internal view returns (uint256) {
    return set._values.length;
}
```

### at(struct StringMap.StringToStringMap,uint256)

- **Kind**: internal
- **Source**: 2072:251:118
- **Link**: `lib/safe-utils/lib/solidity-http/src/StringMap.sol:StringMap:at(struct StringMap.StringToStringMap,uint256)`

```solidity
///  @dev Returns the key-value pair stored at position `index` in the map. O(1).
///  Note that there are no guarantees on the ordering of entries inside the
///  array, and it may change when more entries are added or removed.
///  Requirements:
///  - `index` must be strictly less than {length}.
function at(StringToStringMap storage map, uint256 index) internal view returns (string memory key, string memory value) {
    string memory atKey = map._keys.at(index);
    return (atKey, map._values[atKey]);
}
```

### at(struct StringSet.Set,uint256)

- **Kind**: internal
- **Source**: 3352:124:119
- **Link**: `lib/safe-utils/lib/solidity-http/src/StringSet.sol:StringSet:at(struct StringSet.Set,uint256)`

```solidity
///  @dev Returns the value stored at position `index` in the set. O(1).
///  Note that there are no guarantees on the ordering of values inside the
///  array, and it may change when more values are added or removed.
///  Requirements:
///  - `index` must be strictly less than {length}.
function at(Set storage set, uint256 index) internal view returns (string memory) {
    return set._values[index];
}
```

### toString(enum HTTP.Method)

- **Kind**: internal
- **Source**: 5812:509:117
- **Link**: `lib/safe-utils/lib/solidity-http/src/HTTP.sol:HTTP:toString(enum HTTP.Method)`

```solidity
function toString(Method method) internal pure returns (string memory) {
    if (method == Method.GET) {
        return "GET";
    } else if (method == Method.POST) {
        return "POST";
    } else if (method == Method.PUT) {
        return "PUT";
    } else if (method == Method.DELETE) {
        return "DELETE";
    } else if (method == Method.PATCH) {
        return "PATCH";
    } else {
        revert();
    }
}
```

## External Calls

- **Vm::startBroadcast()**
- **TimelockController::getMinDelay()**
- **Vm::stopBroadcast()**

## State Variable Reads

- **safe** (`struct Safe.Client`)
- **signer** (`address`)
- **derivationPath** (`string`)
- **vm** (`contract Vm`) [lib/forge-std/src/Vm.sol/interface_Vm.md]

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: ProposeSafeTxUpgradeVeryLiquidVaultsScript.run() (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
  ├─ [1] ⚙️ FUNCTION: console.log(string,address) (NodeID: 1)
  │   💬 Args: ["New implementation", address(newImplementation)]
  │   👁️  Def: internal
  │ └─ [2] ⚙️ FUNCTION: StdUtils._sendLogPayload(bytes) (NodeID: 2)
  │     💬 Args: [abi.encodeWithSignature("log(string,address)", p0, p1)]
  │     👁️  Def: internal
  │   └─ [3] ⚙️ FUNCTION: StdUtils._castLogPayloadViewToPure(function (bytes) view) (NodeID: 3)
  │       💬 Args: [_sendLogPayloadView]
  │       👁️  Def: internal
  ├─ [1] ⚙️ FUNCTION: ProposeSafeTxUpgradeVeryLiquidVaultsScript.getMainnetVeryLiquidVaults() (NodeID: 4)
  │   💬 Args: [no args]
  │   👁️  Def: public
  ├─ [1] ⚙️ FUNCTION: ProposeSafeTxUpgradeVeryLiquidVaultsScript.getBaseVeryLiquidVaults() (NodeID: 5)
  │   💬 Args: [no args]
  │   👁️  Def: public
  └─ [1] ⚙️ FUNCTION: Safe.proposeTransactions(struct Safe.Client,address[],bytes[],address,string) (NodeID: 6)
      💬 Args: [safe, multisigTargets, multisigDatas, signer, derivationPath]
      👁️  Def: internal
    ├─ [2] ⚙️ FUNCTION: Safe.getProposeTransactionsTargetAndData(struct Safe.Client,address[],bytes[]) (NodeID: 7)
    │   💬 Args: [self, targets, datas]
    │   👁️  Def: internal
    │ └─ [3] ⚙️ FUNCTION: Safe.getMultiSendCallOnly(struct Safe.Client,uint256) (NodeID: 8)
    │     💬 Args: [self, block.chainid]
    │     👁️  Def: internal
    │   └─ [4] ⚙️ FUNCTION: Safe.instance(struct Safe.Client) (NodeID: 9)
    │       💬 Args: [self]
    │       👁️  Def: internal
    ├─ [2] ⚙️ FUNCTION: Safe.sign(struct Safe.Client,address,bytes,enum Enum.Operation,address,string) (NodeID: 10)
    │   💬 Args: [self, to, data, Enum.Operation.DelegateCall, sender, derivationPath]
    │   👁️  Def: internal
    │ ├─ [3] ⚙️ FUNCTION: Safe.getNonce(struct Safe.Client) (NodeID: 11)
    │ │   💬 Args: [self]
    │ │   👁️  Def: internal
    │ │ └─ [4] ⚙️ FUNCTION: Safe.instance(struct Safe.Client) (NodeID: 12)
    │ │     💬 Args: [self]
    │ │     👁️  Def: internal
    │ ├─ [3] ⚙️ FUNCTION: Safe.instance(struct Safe.Client) (NodeID: 13)
    │ │   💬 Args: [self]
    │ │   👁️  Def: internal
    │ └─ [3] ⚙️ FUNCTION: Safe.getSafeTxHash(struct Safe.Client,address,uint256,bytes,enum Enum.Operation,uint256) (NodeID: 14)
    │     💬 Args: [self, to, 0, data, operation, nonce]
    │     👁️  Def: internal
    │   └─ [4] ⚙️ FUNCTION: Safe.instance(struct Safe.Client) (NodeID: 15)
    │       💬 Args: [self]
    │       👁️  Def: internal
    ├─ [2] ⚙️ FUNCTION: Safe.getNonce(struct Safe.Client) (NodeID: 16)
    │   💬 Args: [self]
    │   👁️  Def: internal
    │ └─ [3] ⚙️ FUNCTION: Safe.instance(struct Safe.Client) (NodeID: 17)
    │     💬 Args: [self]
    │     👁️  Def: internal
    └─ [2] ⚙️ FUNCTION: Safe.proposeTransaction(struct Safe.Client,struct Safe.ExecTransactionParams) (NodeID: 18)
        💬 Args: [self, params]
        👁️  Def: internal
      ├─ [3] ⚙️ FUNCTION: Safe.getSafeTxHash(struct Safe.Client,address,uint256,bytes,enum Enum.Operation,uint256) (NodeID: 19)
      │   💬 Args: [self, params.to, params.value, params.data, params.operation, params.nonce]
      │   👁️  Def: internal
      │ └─ [4] ⚙️ FUNCTION: Safe.instance(struct Safe.Client) (NodeID: 20)
      │     💬 Args: [self]
      │     👁️  Def: internal
      ├─ [3] ⚙️ FUNCTION: Safe.instance(struct Safe.Client) (NodeID: 21)
      │   💬 Args: [self]
      │   👁️  Def: internal
      ├─ [3] ⚙️ FUNCTION: Safe.instance(struct Safe.Client) (NodeID: 22)
      │   💬 Args: [self]
      │   👁️  Def: internal
      ├─ [3] ⚙️ FUNCTION: Safe.instance(struct Safe.Client) (NodeID: 23)
      │   💬 Args: [self]
      │   👁️  Def: internal
      ├─ [3] ⚙️ FUNCTION: Safe.instance(struct Safe.Client) (NodeID: 24)
      │   💬 Args: [self]
      │   👁️  Def: internal
      ├─ [3] ⚙️ FUNCTION: Safe.instance(struct Safe.Client) (NodeID: 25)
      │   💬 Args: [self]
      │   👁️  Def: internal
      ├─ [3] ⚙️ FUNCTION: Safe.instance(struct Safe.Client) (NodeID: 26)
      │   💬 Args: [self]
      │   👁️  Def: internal
      ├─ [3] ⚙️ FUNCTION: Safe.instance(struct Safe.Client) (NodeID: 27)
      │   💬 Args: [self]
      │   👁️  Def: internal
      ├─ [3] ⚙️ FUNCTION: Safe.instance(struct Safe.Client) (NodeID: 28)
      │   💬 Args: [self]
      │   👁️  Def: internal
      ├─ [3] ⚙️ FUNCTION: Safe.instance(struct Safe.Client) (NodeID: 29)
      │   💬 Args: [self]
      │   👁️  Def: internal
      ├─ [3] ⚙️ FUNCTION: Safe.instance(struct Safe.Client) (NodeID: 30)
      │   💬 Args: [self]
      │   👁️  Def: internal
      ├─ [3] ⚙️ FUNCTION: Safe.instance(struct Safe.Client) (NodeID: 31)
      │   💬 Args: [self]
      │   👁️  Def: internal
      ├─ [3] ⚙️ FUNCTION: Safe.instance(struct Safe.Client) (NodeID: 32)
      │   💬 Args: [self]
      │   👁️  Def: internal
      ├─ [3] ⚙️ FUNCTION: HTTP.instance(struct HTTP.Client) (NodeID: 33)
      │   💬 Args: [instance(self).http]
      │   👁️  Def: internal
      ├─ [3] ⚙️ FUNCTION: HTTP.POST(struct HTTP.Request,string) (NodeID: 34)
      │   💬 Args: [instance(self).http.instance(), string.concat(getApiKitUrl(self, block.chainid), "/v1/safes/", vm.toString(instance(self).safe), "/multisig-transactions/")]
      │   👁️  Def: internal
      │ ├─ [4] ⚙️ FUNCTION: Safe.getApiKitUrl(struct Safe.Client,uint256) (NodeID: 38)
      │ │   💬 Args: [self, block.chainid]
      │ │   👁️  Def: internal
      │ │ └─ [5] ⚙️ FUNCTION: Safe.instance(struct Safe.Client) (NodeID: 39)
      │ │     💬 Args: [self]
      │ │     👁️  Def: internal
      │ ├─ [4] ⚙️ FUNCTION: Safe.instance(struct Safe.Client) (NodeID: 40)
      │ │   💬 Args: [self]
      │ │   👁️  Def: internal
      │ └─ [4] ⚙️ FUNCTION: HTTP.POST(struct HTTP.Request) (NodeID: 35)
      │     💬 Args: [withUrl(req, url)]
      │     👁️  Def: internal
      │   ├─ [5] ⚙️ FUNCTION: HTTP.withUrl(struct HTTP.Request,string) (NodeID: 37)
      │   │   💬 Args: [req, url]
      │   │   👁️  Def: internal
      │   └─ [5] ⚙️ FUNCTION: HTTP.withMethod(struct HTTP.Request,enum HTTP.Method) (NodeID: 36)
      │       💬 Args: [req, HTTP.Method.POST]
      │       👁️  Def: internal
      ├─ [3] ⚙️ FUNCTION: HTTP.withBody(struct HTTP.Request,string) (NodeID: 41)
      │   💬 Args: [instance(self).http.instance().POST(string.concat(getApiKitUrl(self, block.chainid), "/v1/safes/", vm.toString(instance(self).safe), "/multisig-transactions/")), instance(self).requestBody]
      │   👁️  Def: internal
      │ └─ [4] ⚙️ FUNCTION: Safe.instance(struct Safe.Client) (NodeID: 42)
      │     💬 Args: [self]
      │     👁️  Def: internal
      └─ [3] ⚙️ FUNCTION: HTTP.request(struct HTTP.Request) (NodeID: 43)
          💬 Args: [instance(self).http.instance().POST(string.concat(getApiKitUrl(self, block.chainid), "/v1/safes/", vm.toString(instance(self).safe), "/multisig-transactions/")).withBody(instance(self).requestBody)]
          👁️  Def: internal
        ├─ [4] ⚙️ FUNCTION: StringMap.length(struct StringMap.StringToStringMap) (NodeID: 44)
        │   💬 Args: [req.headers]
        │   👁️  Def: internal
        │ └─ [5] ⚙️ FUNCTION: StringSet.length(struct StringSet.Set) (NodeID: 45)
        │     💬 Args: [map._keys]
        │     👁️  Def: internal
        ├─ [4] ⚙️ FUNCTION: StringMap.at(struct StringMap.StringToStringMap,uint256) (NodeID: 46)
        │   💬 Args: [req.headers, i]
        │   👁️  Def: internal
        │ └─ [5] ⚙️ FUNCTION: StringSet.at(struct StringSet.Set,uint256) (NodeID: 47)
        │     💬 Args: [map._keys, index]
        │     👁️  Def: internal
        └─ [4] ⚙️ FUNCTION: HTTP.toString(enum HTTP.Method) (NodeID: 48)
            💬 Args: [req.method]
            👁️  Def: internal
```
