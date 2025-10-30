# Function: constructor(contract IPool)

**Contract**: [lib/aave-v3-origin/src/contracts/instances/ATokenInstance.sol/contract_ATokenInstance.md]

## Metadata

- **Contract**: ATokenInstance
- **Signature**: `constructor(contract IPool)`
- **Visibility**: public
- **Source Range**: 297:39:10

## Implementation

```solidity
constructor(IPool pool) AToken(pool) {}
```

## Related Implementations

### (contract IPool)

- **Kind**: internal
- **Source**: 1610:144:31
- **Link**: `lib/aave-v3-origin/src/contracts/protocol/tokenization/AToken.sol:AToken:constructor(contract IPool)`

```solidity
///  @dev Constructor.
///  @param pool The address of the Pool contract
constructor(IPool pool) ScaledBalanceTokenBase(pool,"ATOKEN_IMPL","ATOKEN_IMPL",0) EIP712Base() {}
```

### ()

- **Kind**: internal
- **Source**: 594:49:34
- **Link**: `lib/aave-v3-origin/src/contracts/protocol/tokenization/base/EIP712Base.sol:EIP712Base:constructor()`

```solidity
///  @dev Constructor.
constructor() {
    _chainId = block.chainid;
}
```

### (contract IPool,string,string,uint8)

- **Kind**: internal
- **Source**: 983:195:37
- **Link**: `lib/aave-v3-origin/src/contracts/protocol/tokenization/base/ScaledBalanceTokenBase.sol:ScaledBalanceTokenBase:constructor(contract IPool,string,string,uint8)`

```solidity
///  @dev Constructor.
///  @param pool The reference to the main Pool contract
///  @param name The name of the token
///  @param symbol The symbol of the token
///  @param decimals The number of decimals of the token
constructor(IPool pool, string memory name, string memory symbol, uint8 decimals) MintableIncentivizedERC20(pool,name,symbol,decimals) {}
```

### (contract IPool,string,string,uint8)

- **Kind**: internal
- **Source**: 692:187:36
- **Link**: `lib/aave-v3-origin/src/contracts/protocol/tokenization/base/MintableIncentivizedERC20.sol:MintableIncentivizedERC20:constructor(contract IPool,string,string,uint8)`

```solidity
///  @dev Constructor.
///  @param pool The reference to the main Pool contract
///  @param name The name of the token
///  @param symbol The symbol of the token
///  @param decimals The number of decimals of the token
constructor(IPool pool, string memory name, string memory symbol, uint8 decimals) IncentivizedERC20(pool,name,symbol,decimals) {}
```

### (contract IPool,string,string,uint8)

- **Kind**: internal
- **Source**: 2599:228:35
- **Link**: `lib/aave-v3-origin/src/contracts/protocol/tokenization/base/IncentivizedERC20.sol:IncentivizedERC20:constructor(contract IPool,string,string,uint8)`

```solidity
///  @dev Constructor.
///  @param pool The reference to the main Pool contract
///  @param name_ The name of the token
///  @param symbol_ The symbol of the token
///  @param decimals_ The number of decimals of the token
constructor(IPool pool, string memory name_, string memory symbol_, uint8 decimals_) {
    _addressesProvider = pool.ADDRESSES_PROVIDER();
    _name = name_;
    _symbol = symbol_;
    _decimals = decimals_;
    POOL = pool;
}
```

## State Variable Writes

- **_chainId** (`uint256`)
- **_addressesProvider** (`contract IPoolAddressesProvider`) [lib/aave-v3-origin/src/contracts/interfaces/IPoolAddressesProvider.sol/interface_IPoolAddressesProvider.md]
- **_name** (`string`)
- **_symbol** (`string`)
- **_decimals** (`uint8`)
- **POOL** (`contract IPool`) [lib/aave-v3-origin/src/contracts/interfaces/IPool.sol/interface_IPool.md]

## Call Tree

```
┌─ [0] 🏗️ CONSTRUCTOR: ATokenInstance.constructor(contract IPool) (NodeID: 0)
    💬 Args: [no args]
    🏗️  Contract: ATokenInstance
  └─ [1] 🏗️ CONSTRUCTOR: AToken.constructor(contract IPool) (NodeID: 1)
      💬 Args: [pool]
      🏗️  Contract: AToken
    ├─ [2] 🏗️ CONSTRUCTOR: EIP712Base.constructor() (NodeID: 2)
    │   💬 Args: [no args]
    │   🏗️  Contract: EIP712Base
    └─ [2] 🏗️ CONSTRUCTOR: ScaledBalanceTokenBase.constructor(contract IPool,string,string,uint8) (NodeID: 3)
        💬 Args: [pool, "ATOKEN_IMPL", "ATOKEN_IMPL", 0]
        🏗️  Contract: ScaledBalanceTokenBase
      └─ [3] 🏗️ CONSTRUCTOR: MintableIncentivizedERC20.constructor(contract IPool,string,string,uint8) (NodeID: 4)
          💬 Args: [pool, "ATOKEN_IMPL", "ATOKEN_IMPL", 0]
          🏗️  Contract: MintableIncentivizedERC20
        └─ [4] 🏗️ CONSTRUCTOR: IncentivizedERC20.constructor(contract IPool,string,string,uint8) (NodeID: 5)
            💬 Args: [pool, "ATOKEN_IMPL", "ATOKEN_IMPL", 0]
            🏗️  Contract: IncentivizedERC20
```
