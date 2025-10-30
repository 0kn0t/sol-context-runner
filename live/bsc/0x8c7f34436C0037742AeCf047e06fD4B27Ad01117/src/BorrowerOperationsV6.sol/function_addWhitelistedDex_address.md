# Function: addWhitelistedDex(address)

**Contract**: [src/BorrowerOperationsV6.sol/contract_BorrowerOperationsV6.md]

## Metadata

- **Contract**: BorrowerOperationsV6
- **Signature**: `addWhitelistedDex(address)`
- **Visibility**: external
- **Source Range**: 2717:191:12

## Implementation

```solidity
function addWhitelistedDex(address dex) external onlyAdmin() {
    require(dex != address(0), "Invalid DEX address");
    whitelistedDexes[dex] = true;
    emit DexAdded(dex);
}
```

## Related Implementations

### onlyAdmin()

- **Kind**: modifier
- **Source**: 2249:114:12
- **Link**: `src/BorrowerOperationsV6.sol:BorrowerOperationsV6:onlyAdmin()`

```solidity
modifier onlyAdmin() {
    require(msg.sender == admin, "Only admin can call this function");
    _;
}
```

## State Variable Reads

- **admin** (`address`)

## State Variable Writes

- **whitelistedDexes** (`mapping(address => bool)`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: BorrowerOperationsV6.addWhitelistedDex(address) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: external
  └─ [1] 🔒 MODIFIER: BorrowerOperationsV6.onlyAdmin() (NodeID: 1)
      💬 Args: [no args]
```
