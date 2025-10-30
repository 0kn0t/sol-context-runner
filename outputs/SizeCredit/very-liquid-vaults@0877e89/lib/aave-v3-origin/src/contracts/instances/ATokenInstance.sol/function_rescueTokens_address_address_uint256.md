# Function: rescueTokens(address,address,uint256)

**Contract**: [lib/aave-v3-origin/src/contracts/instances/ATokenInstance.sol/contract_ATokenInstance.md]

## Metadata

- **Contract**: ATokenInstance
- **Signature**: `rescueTokens(address,address,uint256)`
- **Visibility**: external
- **Source Range**: 7315:223:31
- **Inherited From**: AToken

## Implementation

```solidity
/// @inheritdoc IAToken
function rescueTokens(address token, address to, uint256 amount) override external onlyPoolAdmin() {
    require(token != _underlyingAsset, Errors.UNDERLYING_CANNOT_BE_RESCUED);
    IERC20(token).safeTransfer(to, amount);
}
```

## Related Implementations

### onlyPoolAdmin()

- **Kind**: modifier
- **Source**: 1175:194:35
- **Link**: `lib/aave-v3-origin/src/contracts/protocol/tokenization/base/IncentivizedERC20.sol:IncentivizedERC20:onlyPoolAdmin()`

```solidity
///  @dev Only pool admin can call functions marked by this modifier.
modifier onlyPoolAdmin() {
    IACLManager aclManager = IACLManager(_addressesProvider.getACLManager());
    require(aclManager.isPoolAdmin(msg.sender), Errors.CALLER_NOT_POOL_ADMIN);
    _;
}
```

## External Calls

- **IERC20::safeTransfer(contract IERC20,address,uint256)**

## State Variable Reads

- **_underlyingAsset** (`address`)
- **_addressesProvider** (`contract IPoolAddressesProvider`) [lib/aave-v3-origin/src/contracts/interfaces/IPoolAddressesProvider.sol/interface_IPoolAddressesProvider.md]

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: AToken.rescueTokens(address,address,uint256) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: external
  └─ [1] 🔒 MODIFIER: IncentivizedERC20.onlyPoolAdmin() (NodeID: 1)
      💬 Args: [no args]
```

## Documentation

### Function Documentation

@inheritdoc IAToken

### Interface Documentation

 @notice Rescue and transfer tokens locked in this contract
 @param token The address of the token
 @param to The address of the recipient
 @param amount The amount of token to transfer
