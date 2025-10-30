# Function: deploy(uint256,bytes32,bytes)

**Contract**: [script/VeryLiquidVault.s.sol/contract_VeryLiquidVaultScript.md]

## Metadata

- **Contract**: VeryLiquidVaultScript
- **Signature**: `deploy(uint256,bytes32,bytes)`
- **Visibility**: public
- **Source Range**: 612:121:125
- **Inherited From**: BaseScript

## Implementation

```solidity
function deploy(uint256 value, bytes32 salt, bytes memory code) public {
    Create2.deploy(value, salt, code);
}
```

## Related Implementations

### deploy(uint256,bytes32,bytes)

- **Kind**: internal
- **Source**: 1210:847:99
- **Link**: `lib/openzeppelin-contracts/contracts/utils/Create2.sol:Create2:deploy(uint256,bytes32,bytes)`

```solidity
///  @dev Deploys a contract using `CREATE2`. The address where the contract
///  will be deployed can be known in advance via {computeAddress}.
///  The bytecode for a contract can be obtained from Solidity with
///  `type(contractName).creationCode`.
///  Requirements:
///  - `bytecode` must not be empty.
///  - `salt` must have not been used for `bytecode` already.
///  - the factory must have a balance of at least `amount`.
///  - if `amount` is non-zero, `bytecode` must have a `payable` constructor.
function deploy(uint256 amount, bytes32 salt, bytes memory bytecode) internal returns (address addr) {
    if (address(this).balance < amount) {
        revert Errors.InsufficientBalance(address(this).balance, amount);
    }
    if (bytecode.length == 0) {
        revert Create2EmptyBytecode();
    }
    assembly ("memory-safe") {
        addr := create2(amount, add(bytecode, 0x20), mload(bytecode), salt)
        if and(iszero(addr), not(iszero(returndatasize()))) {
            let p := mload(0x40)
            returndatacopy(p, 0, returndatasize())
            revert(p, returndatasize())
        }
    }
    if (addr == address(0)) {
        revert Errors.FailedDeployment();
    }
}
```

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: BaseScript.deploy(uint256,bytes32,bytes) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
  └─ [1] ⚙️ FUNCTION: Create2.deploy(uint256,bytes32,bytes) (NodeID: 1)
      💬 Args: [value, salt, code]
      👁️  Def: internal
```
