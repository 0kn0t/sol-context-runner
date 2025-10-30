# Function: DOMAIN_SEPARATOR()

**Contract**: [lib/aave-v3-origin/src/contracts/instances/VariableDebtTokenInstance.sol/contract_VariableDebtTokenInstance.md]

## Metadata

- **Contract**: VariableDebtTokenInstance
- **Signature**: `DOMAIN_SEPARATOR()`
- **Visibility**: public
- **Source Range**: 862:185:34
- **Inherited From**: EIP712Base

## Implementation

```solidity
///  @notice Get the domain separator for the token
///  @dev Return cached value if chainId matches cache, otherwise recomputes separator
///  @return The domain separator of the token at current chain
function DOMAIN_SEPARATOR() virtual public view returns (bytes32) {
    if (block.chainid == _chainId) {
        return _domainSeparator;
    }
    return _calculateDomainSeparator();
}
```

## Related Implementations

### _calculateDomainSeparator()

- **Kind**: internal
- **Source**: 1470:298:34
- **Link**: `lib/aave-v3-origin/src/contracts/protocol/tokenization/base/EIP712Base.sol:EIP712Base:_calculateDomainSeparator()`

```solidity
///  @notice Compute the current domain separator
///  @return The domain separator for the token
function _calculateDomainSeparator() internal view returns (bytes32) {
    return keccak256(abi.encode(EIP712_DOMAIN, keccak256(bytes(_EIP712BaseId())), keccak256(EIP712_REVISION), block.chainid, address(this)));
}
```

### _EIP712BaseId()

- **Kind**: internal
- **Source**: 3103:96:32
- **Link**: `lib/aave-v3-origin/src/contracts/protocol/tokenization/VariableDebtToken.sol:VariableDebtToken:_EIP712BaseId()`

```solidity
/// @inheritdoc EIP712Base
function _EIP712BaseId() override internal view returns (string memory) {
    return name();
}
```

### name()

- **Kind**: internal
- **Source**: 2864:84:35
- **Link**: `lib/aave-v3-origin/src/contracts/protocol/tokenization/base/IncentivizedERC20.sol:IncentivizedERC20:name()`

```solidity
/// @inheritdoc IERC20Detailed
function name() override public view returns (string memory) {
    return _name;
}
```

## State Variable Reads

- **_chainId** (`uint256`)
- **_domainSeparator** (`bytes32`)
- **EIP712_DOMAIN** (`bytes32`)
- **EIP712_REVISION** (`bytes`)
- **_name** (`string`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: EIP712Base.DOMAIN_SEPARATOR() (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
  └─ [1] ⚙️ FUNCTION: EIP712Base._calculateDomainSeparator() (NodeID: 1)
      💬 Args: [no args]
      👁️  Def: internal
    └─ [2] ⚙️ FUNCTION: VariableDebtToken._EIP712BaseId() (NodeID: 2)
        💬 Args: [no args]
        👁️  Def: internal
      └─ [3] ⚙️ FUNCTION: IncentivizedERC20.name() (NodeID: 3)
          💬 Args: [no args]
          👁️  Def: public
```

## Documentation

### Function Documentation

 @notice Get the domain separator for the token
 @dev Return cached value if chainId matches cache, otherwise recomputes separator
 @return The domain separator of the token at current chain
