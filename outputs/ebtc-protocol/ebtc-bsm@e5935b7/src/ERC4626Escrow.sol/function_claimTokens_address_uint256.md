# Function: claimTokens(address,uint256)

**Contract**: [src/ERC4626Escrow.sol/contract_ERC4626Escrow.md]

## Metadata

- **Contract**: ERC4626Escrow
- **Signature**: `claimTokens(address,uint256)`
- **Visibility**: external
- **Source Range**: 6718:118:22
- **Inherited From**: BaseEscrow

## Implementation

```solidity
/// @notice Claim reward tokens and/or other tokens sent to this contract
function claimTokens(address token, uint256 amount) external requiresAuth() {
    _claimTokens(token, amount);
}
```

## Related Implementations

### _claimTokens(address,uint256)

- **Kind**: internal
- **Source**: 7405:188:39
- **Link**: `src/ERC4626Escrow.sol:ERC4626Escrow:_claimTokens(address,uint256)`

```solidity
function _claimTokens(address token, uint256 amount) override internal {
    require(token != address(EXTERNAL_VAULT), InvalidToken());
    super._claimTokens(token, amount);
}
```

### _claimTokens(address,uint256)

- **Kind**: internal
- **Source**: 6394:240:22
- **Link**: `src/BaseEscrow.sol:BaseEscrow:_claimTokens(address,uint256)`

```solidity
function _claimTokens(address token, uint256 amount) virtual internal {
    require(token != address(ASSET_TOKEN), InvalidToken());
    if (amount > 0) {
        IERC20(token).safeTransfer(FEE_RECIPIENT, amount);
    }
}
```

### requiresAuth()

- **Kind**: modifier
- **Source**: 647:125:25
- **Link**: `src/Dependencies/AuthNoOwner.sol:AuthNoOwner:requiresAuth()`

```solidity
modifier requiresAuth() virtual {
    require(isAuthorized(msg.sender, msg.sig), "Auth: UNAUTHORIZED");
    _;
}
```

### isAuthorized(address,bytes4)

- **Kind**: internal
- **Source**: 981:524:25
- **Link**: `src/Dependencies/AuthNoOwner.sol:AuthNoOwner:isAuthorized(address,bytes4)`

```solidity
function isAuthorized(address user, bytes4 functionSig) virtual internal view returns (bool) {
    Authority auth = _authority;
    return ((address(auth) != address(0)) && auth.canCall(user, address(this), functionSig));
}
```

## State Variable Reads

- **EXTERNAL_VAULT** (`contract IERC4626`) [lib/openzeppelin-contracts/contracts/interfaces/IERC4626.sol/interface_IERC4626.md]
- **ASSET_TOKEN** (`contract IERC20`) [lib/openzeppelin-contracts/contracts/token/ERC20/IERC20.sol/interface_IERC20.md]
- **FEE_RECIPIENT** (`address`)
- **_authority** (`contract Authority`) [src/Dependencies/Authority.sol/interface_Authority.md]

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: BaseEscrow.claimTokens(address,uint256) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: external
  ├─ [1] ⚙️ FUNCTION: ERC4626Escrow._claimTokens(address,uint256) (NodeID: 1)
  │   💬 Args: [token, amount]
  │   👁️  Def: internal
  │ └─ [2] ⚙️ FUNCTION: BaseEscrow._claimTokens(address,uint256) (NodeID: 2)
  │     💬 Args: [token, amount]
  │     👁️  Def: internal
  └─ [1] 🔒 MODIFIER: AuthNoOwner.requiresAuth() (NodeID: 3)
      💬 Args: [no args]
    └─ [2] ⚙️ FUNCTION: AuthNoOwner.isAuthorized(address,bytes4) (NodeID: 4)
        💬 Args: [msg.sender, msg.sig]
        👁️  Def: internal
```

## Documentation

### Function Documentation

@notice Claim reward tokens and/or other tokens sent to this contract
