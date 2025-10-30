# Function: setUp()

**Contract**: [script/ERC4626StrategyVault.s.sol/contract_ERC4626StrategyVaultScript.md]

## Metadata

- **Contract**: ERC4626StrategyVaultScript
- **Signature**: `setUp()`
- **Visibility**: public
- **Source Range**: 823:260:133

## Implementation

```solidity
function setUp() override public {
    super.setUp();
    auth = Auth(vm.envAddress("AUTH"));
    fundingAccount = msg.sender;
    firstDepositAmount = vm.envUint("FIRST_DEPOSIT_AMOUNT");
    vault = IERC4626(vm.envAddress("VAULT"));
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
- **vault** (`contract IERC4626`) [lib/openzeppelin-contracts/contracts/interfaces/IERC4626.sol/interface_IERC4626.md]
- **create2Deployer** (`contract ICreate2Deployer`) [script/ICreate2Deployer.s.sol/interface_ICreate2Deployer.md]

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: ERC4626StrategyVaultScript.setUp() (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
  └─ [1] ⚙️ FUNCTION: BaseScript.setUp() (NodeID: 1)
      💬 Args: [no args]
      👁️  Def: public
```
