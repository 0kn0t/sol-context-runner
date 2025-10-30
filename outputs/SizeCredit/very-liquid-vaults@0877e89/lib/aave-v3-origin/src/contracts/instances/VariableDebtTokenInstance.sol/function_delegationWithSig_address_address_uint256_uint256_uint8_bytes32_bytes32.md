# Function: delegationWithSig(address,address,uint256,uint256,uint8,bytes32,bytes32)

**Contract**: [lib/aave-v3-origin/src/contracts/instances/VariableDebtTokenInstance.sol/contract_VariableDebtTokenInstance.md]

## Metadata

- **Contract**: VariableDebtTokenInstance
- **Signature**: `delegationWithSig(address,address,uint256,uint256,uint8,bytes32,bytes32)`
- **Visibility**: external
- **Source Range**: 1398:823:33
- **Inherited From**: DebtTokenBase

## Implementation

```solidity
/// @inheritdoc ICreditDelegationToken
function delegationWithSig(address delegator, address delegatee, uint256 value, uint256 deadline, uint8 v, bytes32 r, bytes32 s) external {
    require(delegator != address(0), Errors.ZERO_ADDRESS_NOT_VALID);
    require(block.timestamp <= deadline, Errors.INVALID_EXPIRATION);
    uint256 currentValidNonce = _nonces[delegator];
    bytes32 digest = keccak256(abi.encodePacked("\u0019\u0001", DOMAIN_SEPARATOR(), keccak256(abi.encode(DELEGATION_WITH_SIG_TYPEHASH, delegatee, value, currentValidNonce, deadline))));
    require(delegator == ecrecover(digest, v, r, s), Errors.INVALID_SIGNATURE);
    _nonces[delegator] = currentValidNonce + 1;
    _approveDelegation(delegator, delegatee, value);
}
```

## Related Implementations

### DOMAIN_SEPARATOR()

- **Kind**: internal
- **Source**: 862:185:34
- **Link**: `lib/aave-v3-origin/src/contracts/protocol/tokenization/base/EIP712Base.sol:EIP712Base:DOMAIN_SEPARATOR()`

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

### _approveDelegation(address,address,uint256)

- **Kind**: internal
- **Source**: 2723:233:33
- **Link**: `lib/aave-v3-origin/src/contracts/protocol/tokenization/base/DebtTokenBase.sol:DebtTokenBase:_approveDelegation(address,address,uint256)`

```solidity
///  @notice Updates the borrow allowance of a user on the specific debt token.
///  @param delegator The address delegating the borrowing power
///  @param delegatee The address receiving the delegated borrowing power
///  @param amount The allowance amount being delegated.
function _approveDelegation(address delegator, address delegatee, uint256 amount) internal {
    _borrowAllowances[delegator][delegatee] = amount;
    emit BorrowAllowanceDelegated(delegator, delegatee, _underlyingAsset, amount);
}
```

## State Variable Reads

- **DELEGATION_WITH_SIG_TYPEHASH** (`bytes32`)
- **_chainId** (`uint256`)
- **_domainSeparator** (`bytes32`)
- **EIP712_DOMAIN** (`bytes32`)
- **EIP712_REVISION** (`bytes`)
- **_name** (`string`)
- **_underlyingAsset** (`address`)

## State Variable Writes

- **_borrowAllowances** (`mapping(address => mapping(address => uint256))`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: DebtTokenBase.delegationWithSig(address,address,uint256,uint256,uint8,bytes32,bytes32) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: external
  ├─ [1] ⚙️ FUNCTION: EIP712Base.DOMAIN_SEPARATOR() (NodeID: 1)
  │   💬 Args: [no args]
  │   👁️  Def: public
  │ └─ [2] ⚙️ FUNCTION: EIP712Base._calculateDomainSeparator() (NodeID: 2)
  │     💬 Args: [no args]
  │     👁️  Def: internal
  │   └─ [3] ⚙️ FUNCTION: VariableDebtToken._EIP712BaseId() (NodeID: 3)
  │       💬 Args: [no args]
  │       👁️  Def: internal
  │     └─ [4] ⚙️ FUNCTION: IncentivizedERC20.name() (NodeID: 4)
  │         💬 Args: [no args]
  │         👁️  Def: public
  └─ [1] ⚙️ FUNCTION: DebtTokenBase._approveDelegation(address,address,uint256) (NodeID: 5)
      💬 Args: [delegator, delegatee, value]
      👁️  Def: internal
```

## Documentation

### Function Documentation

@inheritdoc ICreditDelegationToken

### Interface Documentation

 @notice Delegates borrowing power to a user on the specific debt token via ERC712 signature
 @param delegator The delegator of the credit
 @param delegatee The delegatee that can use the credit
 @param value The amount to be delegated
 @param deadline The deadline timestamp, type(uint256).max for max deadline
 @param v The V signature param
 @param s The S signature param
 @param r The R signature param
