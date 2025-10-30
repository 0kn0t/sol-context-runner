# Function: getBalance()

**Contract**: [src/TokenHolder.sol/contract_TokenHolder.md]

## Metadata

- **Contract**: TokenHolder
- **Signature**: `getBalance()`
- **Visibility**: public
- **Source Range**: 2913:112:13

## Implementation

```solidity
function getBalance() public view returns (uint256) {
    return tokenHolded.balanceOf(address(this));
}
```

## External Calls

- **IERC20::balanceOf(address)**

## State Variable Reads

- **tokenHolded** (`contract IERC20`) [lib/openzeppelin-contracts-upgradeable/lib/openzeppelin-contracts/contracts/token/ERC20/IERC20.sol/interface_IERC20.md]

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: TokenHolder.getBalance() (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
```
