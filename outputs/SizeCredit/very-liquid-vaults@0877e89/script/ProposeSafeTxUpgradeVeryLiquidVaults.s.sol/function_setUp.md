# Function: setUp()

**Contract**: [script/ProposeSafeTxUpgradeVeryLiquidVaults.s.sol/contract_ProposeSafeTxUpgradeVeryLiquidVaultsScript.md]

## Metadata

- **Contract**: ProposeSafeTxUpgradeVeryLiquidVaultsScript
- **Signature**: `setUp()`
- **Visibility**: public
- **Source Range**: 696:243:138

## Implementation

```solidity
function setUp() override public {
    super.setUp();
    signer = vm.envAddress("SIGNER");
    derivationPath = vm.envString("DERIVATION_PATH");
    safe.initialize(addresses[block.chainid][Contract.GovernanceMultisig]);
}
```

## Related Implementations

### setUp()

- **Kind**: internal
- **Source**: 416:190:125
- **Link**: `script/BaseScript.s.sol:BaseScript:setUp()`

```solidity
function setUp() virtual public {
    create2Deployer = ICreate2Deployer(0x13b0D85CcB8bf860b6b79AF3029fCA081AE9beF2);
    vm.label(address(create2Deployer), "Create2Deployer");
}
```

### initialize(struct Safe.Client,address)

- **Kind**: internal
- **Source**: 1942:4114:121
- **Link**: `lib/safe-utils/src/Safe.sol:Safe:initialize(struct Safe.Client,address)`

```solidity
function initialize(Client storage self, address safe) internal returns (Client storage) {
    self.instances.push();
    Instance storage i = self.instances[self.instances.length - 1];
    i.safe = safe;
    i.urls[1] = "https://safe-transaction-mainnet.safe.global/api";
    i.urls[10] = "https://safe-transaction-optimism.safe.global/api";
    i.urls[56] = "https://safe-transaction-bsc.safe.global/api";
    i.urls[100] = "https://safe-transaction-gnosis-chain.safe.global/api";
    i.urls[130] = "https://safe-transaction-unichain.safe.global/api";
    i.urls[137] = "https://safe-transaction-polygon.safe.global/api";
    i.urls[196] = "https://safe-transaction-xlayer.safe.global/api";
    i.urls[324] = "https://safe-transaction-zksync.safe.global/api";
    i.urls[480] = "https://safe-transaction-worldchain.safe.global/api";
    i.urls[1101] = "https://safe-transaction-zkevm.safe.global/api";
    i.urls[5000] = "https://safe-transaction-mantle.safe.global/api";
    i.urls[8453] = "https://safe-transaction-base.safe.global/api";
    i.urls[42161] = "https://safe-transaction-arbitrum.safe.global/api";
    i.urls[42220] = "https://safe-transaction-celo.safe.global/api";
    i.urls[43114] = "https://safe-transaction-avalanche.safe.global/api";
    i.urls[59144] = "https://safe-transaction-linea.safe.global/api";
    i.urls[81457] = "https://safe-transaction-blast.safe.global/api";
    i.urls[84532] = "https://safe-transaction-base-sepolia.safe.global/api";
    i.urls[534352] = "https://safe-transaction-scroll.safe.global/api";
    i.urls[11155111] = "https://safe-transaction-sepolia.safe.global/api";
    i.urls[1313161554] = "https://safe-transaction-aurora.safe.global/api";
    i.multiSendCallOnly[1] = MultiSendCallOnly(MULTI_SEND_CALL_ONLY_ADDRESS_CANONICAL);
    i.multiSendCallOnly[10] = MultiSendCallOnly(MULTI_SEND_CALL_ONLY_ADDRESS_CANONICAL);
    i.multiSendCallOnly[56] = MultiSendCallOnly(MULTI_SEND_CALL_ONLY_ADDRESS_CANONICAL);
    i.multiSendCallOnly[100] = MultiSendCallOnly(MULTI_SEND_CALL_ONLY_ADDRESS_CANONICAL);
    i.multiSendCallOnly[130] = MultiSendCallOnly(MULTI_SEND_CALL_ONLY_ADDRESS_CANONICAL);
    i.multiSendCallOnly[137] = MultiSendCallOnly(MULTI_SEND_CALL_ONLY_ADDRESS_CANONICAL);
    i.multiSendCallOnly[196] = MultiSendCallOnly(MULTI_SEND_CALL_ONLY_ADDRESS_CANONICAL);
    i.multiSendCallOnly[324] = MultiSendCallOnly(MULTI_SEND_CALL_ONLY_ADDRESS_ZKSYNC);
    i.multiSendCallOnly[480] = MultiSendCallOnly(MULTI_SEND_CALL_ONLY_ADDRESS_CANONICAL);
    i.multiSendCallOnly[1101] = MultiSendCallOnly(MULTI_SEND_CALL_ONLY_ADDRESS_CANONICAL);
    i.multiSendCallOnly[5000] = MultiSendCallOnly(MULTI_SEND_CALL_ONLY_ADDRESS_CANONICAL);
    i.multiSendCallOnly[8453] = MultiSendCallOnly(MULTI_SEND_CALL_ONLY_ADDRESS_CANONICAL);
    i.multiSendCallOnly[42161] = MultiSendCallOnly(MULTI_SEND_CALL_ONLY_ADDRESS_CANONICAL);
    i.multiSendCallOnly[42220] = MultiSendCallOnly(MULTI_SEND_CALL_ONLY_ADDRESS_CANONICAL);
    i.multiSendCallOnly[43114] = MultiSendCallOnly(MULTI_SEND_CALL_ONLY_ADDRESS_CANONICAL);
    i.multiSendCallOnly[59144] = MultiSendCallOnly(MULTI_SEND_CALL_ONLY_ADDRESS_CANONICAL);
    i.multiSendCallOnly[81457] = MultiSendCallOnly(MULTI_SEND_CALL_ONLY_ADDRESS_CANONICAL);
    i.multiSendCallOnly[84532] = MultiSendCallOnly(MULTI_SEND_CALL_ONLY_ADDRESS_CANONICAL);
    i.multiSendCallOnly[534352] = MultiSendCallOnly(MULTI_SEND_CALL_ONLY_ADDRESS_CANONICAL);
    i.multiSendCallOnly[11155111] = MultiSendCallOnly(MULTI_SEND_CALL_ONLY_ADDRESS_CANONICAL);
    i.multiSendCallOnly[1313161554] = MultiSendCallOnly(MULTI_SEND_CALL_ONLY_ADDRESS_CANONICAL);
    i.http.initialize().withHeader("Content-Type", "application/json");
    return self;
}
```

### initialize(struct HTTP.Client)

- **Kind**: internal
- **Source**: 767:187:117
- **Link**: `lib/safe-utils/lib/solidity-http/src/HTTP.sol:HTTP:initialize(struct HTTP.Client)`

```solidity
function initialize(HTTP.Client storage client) internal returns (HTTP.Request storage) {
    client.requests.push();
    return client.requests[client.requests.length - 1];
}
```

### withHeader(struct HTTP.Request,string,string)

- **Kind**: internal
- **Source**: 3288:210:117
- **Link**: `lib/safe-utils/lib/solidity-http/src/HTTP.sol:HTTP:withHeader(struct HTTP.Request,string,string)`

```solidity
function withHeader(HTTP.Request storage req, string memory key, string memory value) internal returns (HTTP.Request storage) {
    req.headers.set(key, value);
    return req;
}
```

### set(struct StringMap.StringToStringMap,string,string)

- **Kind**: internal
- **Source**: 765:184:118
- **Link**: `lib/safe-utils/lib/solidity-http/src/StringMap.sol:StringMap:set(struct StringMap.StringToStringMap,string,string)`

```solidity
///  @dev Adds a key-value pair to a map, or updates the value for an existing
///  key. O(1).
///  Returns true if the key was added to the map, that is if it was not
///  already present.
function set(StringToStringMap storage map, string memory key, string memory value) internal returns (bool) {
    map._values[key] = value;
    return map._keys.add(key);
}
```

### add(struct StringSet.Set,string)

- **Kind**: internal
- **Source**: 647:411:119
- **Link**: `lib/safe-utils/lib/solidity-http/src/StringSet.sol:StringSet:add(struct StringSet.Set,string)`

```solidity
///  @dev Add a value to a set. O(1).
///  Returns true if the value was added to the set, that is if it was not
///  already present.
function add(Set storage set, string memory value) internal returns (bool) {
    if (!contains(set, value)) {
        set._values.push(value);
        set._positions[value] = set._values.length;
        return true;
    } else {
        return false;
    }
}
```

### contains(struct StringSet.Set,string)

- **Kind**: internal
- **Source**: 2687:135:119
- **Link**: `lib/safe-utils/lib/solidity-http/src/StringSet.sol:StringSet:contains(struct StringSet.Set,string)`

```solidity
///  @dev Returns true if the value is in the set. O(1).
function contains(Set storage set, string memory value) internal view returns (bool) {
    return set._positions[value] != 0;
}
```

## External Calls

- **Vm::envAddress(string)**
- **Vm::envString(string)**

## State Variable Reads

- **safe** (`struct Safe.Client`)
- **create2Deployer** (`contract ICreate2Deployer`) [script/ICreate2Deployer.s.sol/interface_ICreate2Deployer.md]
- **MULTI_SEND_CALL_ONLY_ADDRESS_CANONICAL** (`address`)
- **MULTI_SEND_CALL_ONLY_ADDRESS_ZKSYNC** (`address`)

## State Variable Writes

- **signer** (`address`)
- **derivationPath** (`string`)
- **create2Deployer** (`contract ICreate2Deployer`) [script/ICreate2Deployer.s.sol/interface_ICreate2Deployer.md]

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: ProposeSafeTxUpgradeVeryLiquidVaultsScript.setUp() (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
  ├─ [1] ⚙️ FUNCTION: BaseScript.setUp() (NodeID: 1)
  │   💬 Args: [no args]
  │   👁️  Def: public
  └─ [1] ⚙️ FUNCTION: Safe.initialize(struct Safe.Client,address) (NodeID: 2)
      💬 Args: [safe, addresses[block.chainid][Contract.GovernanceMultisig]]
      👁️  Def: internal
    ├─ [2] ⚙️ FUNCTION: HTTP.initialize(struct HTTP.Client) (NodeID: 3)
    │   💬 Args: [i.http]
    │   👁️  Def: internal
    └─ [2] ⚙️ FUNCTION: HTTP.withHeader(struct HTTP.Request,string,string) (NodeID: 4)
        💬 Args: [i.http.initialize(), "Content-Type", "application/json"]
        👁️  Def: internal
      └─ [3] ⚙️ FUNCTION: StringMap.set(struct StringMap.StringToStringMap,string,string) (NodeID: 5)
          💬 Args: [req.headers, key, value]
          👁️  Def: internal
        └─ [4] ⚙️ FUNCTION: StringSet.add(struct StringSet.Set,string) (NodeID: 6)
            💬 Args: [map._keys, key]
            👁️  Def: internal
          └─ [5] ⚙️ FUNCTION: StringSet.contains(struct StringSet.Set,string) (NodeID: 7)
              💬 Args: [set, value]
              👁️  Def: internal
```
