# Function: computeAddress(bytes32,bytes32)

**Contract**: [script/CashStrategyVault.s.sol/contract_CashStrategyVaultScript.md]

## Metadata

- **Contract**: CashStrategyVaultScript
- **Signature**: `computeAddress(bytes32,bytes32)`
- **Visibility**: public
- **Source Range**: 739:148:125
- **Inherited From**: BaseScript

## Implementation

```solidity
function computeAddress(bytes32 salt, bytes32 codeHash) public view returns (address) {
    return Create2.computeAddress(salt, codeHash);
}
```

## Related Implementations

### computeAddress(bytes32,bytes32)

- **Kind**: internal
- **Source**: 2261:165:99
- **Link**: `lib/openzeppelin-contracts/contracts/utils/Create2.sol:Create2:computeAddress(bytes32,bytes32)`

```solidity
///  @dev Returns the address where a contract will be stored if deployed via {deploy}. Any change in the
///  `bytecodeHash` or `salt` will result in a new destination address.
function computeAddress(bytes32 salt, bytes32 bytecodeHash) internal view returns (address) {
    return computeAddress(salt, bytecodeHash, address(this));
}
```

### computeAddress(bytes32,bytes32,address)

- **Kind**: internal
- **Source**: 2669:1794:99
- **Link**: `lib/openzeppelin-contracts/contracts/utils/Create2.sol:Create2:computeAddress(bytes32,bytes32,address)`

```solidity
///  @dev Returns the address where a contract will be stored if deployed via {deploy} from a contract located at
///  `deployer`. If `deployer` is this contract's address, returns the same value as {computeAddress}.
function computeAddress(bytes32 salt, bytes32 bytecodeHash, address deployer) internal pure returns (address addr) {
    assembly ("memory-safe") {
        let ptr := mload(0x40)
        mstore(add(ptr, 0x40), bytecodeHash)
        mstore(add(ptr, 0x20), salt)
        mstore(ptr, deployer)
        let start := add(ptr, 0x0b)
        mstore8(start, 0xff)
        addr := and(keccak256(start, 85), 0xffffffffffffffffffffffffffffffffffffffff)
    }
}
```

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: BaseScript.computeAddress(bytes32,bytes32) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
  └─ [1] ⚙️ FUNCTION: Create2.computeAddress(bytes32,bytes32) (NodeID: 1)
      💬 Args: [salt, codeHash]
      👁️  Def: internal
    └─ [2] ⚙️ FUNCTION: Create2.computeAddress(bytes32,bytes32,address) (NodeID: 2)
        💬 Args: [salt, bytecodeHash, address(this)]
        👁️  Def: internal
```
