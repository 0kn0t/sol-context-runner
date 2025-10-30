# Function: constructor(contract IMorpho)

**Contract**: [src/mocks/FlashBorrowerMock.sol/contract_FlashBorrowerMock.md]

## Metadata

- **Contract**: FlashBorrowerMock
- **Signature**: `constructor(contract IMorpho)`
- **Visibility**: public
- **Source Range**: 347:66:18

## Implementation

```solidity
constructor(IMorpho newMorpho) {
    MORPHO = newMorpho;
}
```

## State Variable Writes

- **MORPHO** (`contract IMorpho`) [src/interfaces/IMorpho.sol/interface_IMorpho.md]

## Call Tree

```
┌─ [0] 🏗️ CONSTRUCTOR: FlashBorrowerMock.constructor(contract IMorpho) (NodeID: 0)
    💬 Args: [no args]
    🏗️  Contract: FlashBorrowerMock
```
