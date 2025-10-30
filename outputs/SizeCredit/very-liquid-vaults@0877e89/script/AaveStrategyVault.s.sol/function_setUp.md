# Function: setUp()

**Contract**: [script/AaveStrategyVault.s.sol/contract_AaveStrategyVaultScript.md]

## Metadata

- **Contract**: AaveStrategyVaultScript
- **Signature**: `setUp()`
- **Visibility**: public
- **Source Range**: 822:311:122

## Implementation

```solidity
function setUp() override public {
    super.setUp();
    auth = Auth(vm.envAddress("AUTH"));
    asset = IERC20Metadata(vm.envAddress("ASSET"));
    fundingAccount = msg.sender;
    firstDepositAmount = vm.envUint("FIRST_DEPOSIT_AMOUNT");
    pool = IPool(vm.envAddress("POOL"));
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
- **asset** (`contract IERC20Metadata`) [lib/openzeppelin-contracts/contracts/token/ERC20/extensions/IERC20Metadata.sol/interface_IERC20Metadata.md]
- **fundingAccount** (`address`)
- **firstDepositAmount** (`uint256`)
- **pool** (`contract IPool`) [lib/aave-v3-origin/src/contracts/interfaces/IPool.sol/interface_IPool.md]
- **create2Deployer** (`contract ICreate2Deployer`) [script/ICreate2Deployer.s.sol/interface_ICreate2Deployer.md]

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: AaveStrategyVaultScript.setUp() (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
  └─ [1] ⚙️ FUNCTION: BaseScript.setUp() (NodeID: 1)
      💬 Args: [no args]
      👁️  Def: public
```
