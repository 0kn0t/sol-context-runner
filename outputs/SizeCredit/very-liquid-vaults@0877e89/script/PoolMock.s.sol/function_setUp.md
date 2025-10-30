# Function: setUp()

**Contract**: [script/PoolMock.s.sol/contract_PoolMockScript.md]

## Metadata

- **Contract**: PoolMockScript
- **Signature**: `setUp()`
- **Visibility**: public
- **Source Range**: 490:127:135

## Implementation

```solidity
function setUp() public {
    owner = vm.envAddress("OWNER");
    asset = IERC20Metadata(vm.envAddress("ASSET"));
}
```

## External Calls

- **Vm::envAddress(string)**

## State Variable Writes

- **owner** (`address`)
- **asset** (`contract IERC20Metadata`) [lib/openzeppelin-contracts/contracts/token/ERC20/extensions/IERC20Metadata.sol/interface_IERC20Metadata.md]

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: PoolMockScript.setUp() (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
```
