# Function: setUp()

**Contract**: [script/VaultMock.s.sol/contract_VaultMockScript.md]

## Metadata

- **Contract**: VaultMockScript
- **Signature**: `setUp()`
- **Visibility**: public
- **Source Range**: 428:197:141

## Implementation

```solidity
function setUp() public {
    owner = vm.envAddress("OWNER");
    asset = IERC20(vm.envAddress("ASSET"));
    name = vm.envString("NAME");
    symbol = vm.envString("SYMBOL");
}
```

## External Calls

- **Vm::envAddress(string)**
- **Vm::envString(string)**

## State Variable Writes

- **owner** (`address`)
- **asset** (`contract IERC20`) [lib/openzeppelin-contracts/contracts/token/ERC20/IERC20.sol/interface_IERC20.md]
- **name** (`string`)
- **symbol** (`string`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: VaultMockScript.setUp() (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
```
