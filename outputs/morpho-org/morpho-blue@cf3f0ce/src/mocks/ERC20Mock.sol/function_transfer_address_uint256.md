# Function: transfer(address,uint256)

**Contract**: [src/mocks/ERC20Mock.sol/contract_ERC20Mock.md]

## Metadata

- **Contract**: ERC20Mock
- **Signature**: `transfer(address,uint256)`
- **Visibility**: public
- **Source Range**: 810:302:17

## Implementation

```solidity
function transfer(address to, uint256 amount) virtual public returns (bool) {
    require(balanceOf[msg.sender] >= amount, "insufficient balance");
    balanceOf[msg.sender] -= amount;
    balanceOf[to] += amount;
    emit Transfer(msg.sender, to, amount);
    return true;
}
```

## State Variable Reads

- **balanceOf** (`mapping(address => uint256)`)

## State Variable Writes

- **balanceOf** (`mapping(address => uint256)`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: ERC20Mock.transfer(address,uint256) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
```
