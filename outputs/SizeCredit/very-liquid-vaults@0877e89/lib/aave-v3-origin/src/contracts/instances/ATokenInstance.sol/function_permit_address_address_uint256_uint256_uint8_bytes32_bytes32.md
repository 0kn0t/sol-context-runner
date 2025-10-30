# Function: permit(address,address,uint256,uint256,uint8,bytes32,bytes32)

**Contract**: [lib/aave-v3-origin/src/contracts/instances/ATokenInstance.sol/contract_ATokenInstance.md]

## Metadata

- **Contract**: ATokenInstance
- **Signature**: `permit(address,address,uint256,uint256,uint8,bytes32,bytes32)`
- **Visibility**: external
- **Source Range**: 4518:755:31
- **Inherited From**: AToken

## Implementation

```solidity
/// @inheritdoc IAToken
function permit(address owner, address spender, uint256 value, uint256 deadline, uint8 v, bytes32 r, bytes32 s) override external {
    require(owner != address(0), Errors.ZERO_ADDRESS_NOT_VALID);
    require(block.timestamp <= deadline, Errors.INVALID_EXPIRATION);
    uint256 currentValidNonce = _nonces[owner];
    bytes32 digest = keccak256(abi.encodePacked("\u0019\u0001", DOMAIN_SEPARATOR(), keccak256(abi.encode(PERMIT_TYPEHASH, owner, spender, value, currentValidNonce, deadline))));
    require(owner == ecrecover(digest, v, r, s), Errors.INVALID_SIGNATURE);
    _nonces[owner] = currentValidNonce + 1;
    _approve(owner, spender, value);
}
```

## Related Implementations

### DOMAIN_SEPARATOR()

- **Kind**: internal
- **Source**: 6749:130:31
- **Link**: `lib/aave-v3-origin/src/contracts/protocol/tokenization/AToken.sol:AToken:DOMAIN_SEPARATOR()`

```solidity
///  @dev Overrides the base function to fully implement IAToken
///  @dev see `EIP712Base.DOMAIN_SEPARATOR()` for more detailed documentation
function DOMAIN_SEPARATOR() override(IAToken, EIP712Base) public view returns (bytes32) {
    return super.DOMAIN_SEPARATOR();
}
```

### _approve(address,address,uint256)

- **Kind**: internal
- **Source**: 7169:173:35
- **Link**: `lib/aave-v3-origin/src/contracts/protocol/tokenization/base/IncentivizedERC20.sol:IncentivizedERC20:_approve(address,address,uint256)`

```solidity
///  @notice Approve `spender` to use `amount` of `owner`s balance
///  @param owner The address owning the tokens
///  @param spender The address approved for spending
///  @param amount The amount of tokens to approve spending of
function _approve(address owner, address spender, uint256 amount) virtual internal {
    _allowances[owner][spender] = amount;
    emit Approval(owner, spender, amount);
}
```

## State Variable Reads

- **PERMIT_TYPEHASH** (`bytes32`)

## State Variable Writes

- **_allowances** (`mapping(address => mapping(address => uint256))`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: AToken.permit(address,address,uint256,uint256,uint8,bytes32,bytes32) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: external
  ├─ [1] ⚙️ FUNCTION: AToken.DOMAIN_SEPARATOR() (NodeID: 1)
  │   💬 Args: [no args]
  │   👁️  Def: public
  └─ [1] ⚙️ FUNCTION: IncentivizedERC20._approve(address,address,uint256) (NodeID: 2)
      💬 Args: [owner, spender, value]
      👁️  Def: internal
```

## Documentation

### Function Documentation

@inheritdoc IAToken

### Interface Documentation

 @notice Allow passing a signed message to approve spending
 @dev implements the permit function as for
 https://github.com/ethereum/EIPs/blob/8a34d644aacf0f9f8f00815307fd7dd5da07655f/EIPS/eip-2612.md
 @param owner The owner of the funds
 @param spender The spender
 @param value The amount
 @param deadline The deadline timestamp, type(uint256).max for max deadline
 @param v Signature param
 @param s Signature param
 @param r Signature param
