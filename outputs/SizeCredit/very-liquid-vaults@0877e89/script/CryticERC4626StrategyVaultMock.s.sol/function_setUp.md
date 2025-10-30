# Function: setUp()

**Contract**: [script/CryticERC4626StrategyVaultMock.s.sol/contract_CryticERC4626StrategyVaultMockScript.md]

## Metadata

- **Contract**: CryticERC4626StrategyVaultMockScript
- **Signature**: `setUp()`
- **Visibility**: public
- **Source Range**: 971:261:131

## Implementation

```solidity
function setUp() override public {
    super.setUp();
    auth = Auth(vm.envAddress("AUTH"));
    fundingAccount = msg.sender;
    firstDepositAmount = vm.envUint("FIRST_DEPOSIT_AMOUNT");
    vault = VaultMock(vm.envAddress("VAULT"));
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

## External Calls

- **Vm::envAddress(string)**
- **Vm::envUint(string)**

## State Variable Reads

- **create2Deployer** (`contract ICreate2Deployer`) [script/ICreate2Deployer.s.sol/interface_ICreate2Deployer.md]

## State Variable Writes

- **auth** (`contract Auth`) [src/Auth.sol/contract_Auth.md]
- **fundingAccount** (`address`)
- **firstDepositAmount** (`uint256`)
- **vault** (`contract VaultMock`)
- **create2Deployer** (`contract ICreate2Deployer`) [script/ICreate2Deployer.s.sol/interface_ICreate2Deployer.md]

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: CryticERC4626StrategyVaultMockScript.setUp() (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
  └─ [1] ⚙️ FUNCTION: BaseScript.setUp() (NodeID: 1)
      💬 Args: [no args]
      👁️  Def: public
```
