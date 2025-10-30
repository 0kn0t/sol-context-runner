# Function: setUp()

**Contract**: [script/VeryLiquidVault.s.sol/contract_VeryLiquidVaultScript.md]

## Metadata

- **Contract**: VeryLiquidVaultScript
- **Signature**: `setUp()`
- **Visibility**: public
- **Source Range**: 897:597:142

## Implementation

```solidity
function setUp() override public {
    super.setUp();
    identifier = vm.envString("IDENTIFIER");
    auth = Auth(vm.envAddress("AUTH"));
    asset = IERC20Metadata(vm.envAddress("ASSET"));
    fundingAccount = msg.sender;
    veryLiquidVaultFirstDepositAmount = vm.envUint("VERY_LIQUID_VAULT_FIRST_DEPOSIT_AMOUNT");
    address[] memory strategies_ = vm.envAddress("STRATEGIES", ",");
    strategies = new IVault[](strategies_.length);
    for (uint256 i = 0; i < strategies_.length; i++) {
        strategies[i] = IVault(strategies_[i]);
    }
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

- **Vm::envString(string)**
- **Vm::envAddress(string)**
- **Vm::envUint(string)**
- **Vm::envAddress(string,string)**

## State Variable Reads

- **create2Deployer** (`contract ICreate2Deployer`) [script/ICreate2Deployer.s.sol/interface_ICreate2Deployer.md]

## State Variable Writes

- **identifier** (`string`)
- **auth** (`contract Auth`) [src/Auth.sol/contract_Auth.md]
- **asset** (`contract IERC20Metadata`) [lib/openzeppelin-contracts/contracts/token/ERC20/extensions/IERC20Metadata.sol/interface_IERC20Metadata.md]
- **fundingAccount** (`address`)
- **veryLiquidVaultFirstDepositAmount** (`uint256`)
- **strategies** (`contract IVault[]`) [src/IVault.sol/interface_IVault.md]
- **create2Deployer** (`contract ICreate2Deployer`) [script/ICreate2Deployer.s.sol/interface_ICreate2Deployer.md]

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: VeryLiquidVaultScript.setUp() (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
  └─ [1] ⚙️ FUNCTION: BaseScript.setUp() (NodeID: 1)
      💬 Args: [no args]
      👁️  Def: public
```
