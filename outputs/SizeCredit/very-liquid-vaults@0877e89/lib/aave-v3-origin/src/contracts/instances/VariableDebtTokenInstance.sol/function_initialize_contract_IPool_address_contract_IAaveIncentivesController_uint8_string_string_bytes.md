# Function: initialize(contract IPool,address,contract IAaveIncentivesController,uint8,string,string,bytes)

**Contract**: [lib/aave-v3-origin/src/contracts/instances/VariableDebtTokenInstance.sol/contract_VariableDebtTokenInstance.md]

## Metadata

- **Contract**: VariableDebtTokenInstance
- **Signature**: `initialize(contract IPool,address,contract IAaveIncentivesController,uint8,string,string,bytes)`
- **Visibility**: external
- **Source Range**: 598:803:11

## Implementation

```solidity
/// @inheritdoc IInitializableDebtToken
function initialize(IPool initializingPool, address underlyingAsset, IAaveIncentivesController incentivesController, uint8 debtTokenDecimals, string memory debtTokenName, string memory debtTokenSymbol, bytes calldata params) override external initializer() {
    require(initializingPool == POOL, Errors.POOL_ADDRESSES_DO_NOT_MATCH);
    _setName(debtTokenName);
    _setSymbol(debtTokenSymbol);
    _setDecimals(debtTokenDecimals);
    _underlyingAsset = underlyingAsset;
    _incentivesController = incentivesController;
    _domainSeparator = _calculateDomainSeparator();
    emit Initialized(underlyingAsset, address(POOL), address(incentivesController), debtTokenDecimals, debtTokenName, debtTokenSymbol, params);
}
```

## Related Implementations

### _setName(string)

- **Kind**: internal
- **Source**: 7447:76:35
- **Link**: `lib/aave-v3-origin/src/contracts/protocol/tokenization/base/IncentivizedERC20.sol:IncentivizedERC20:_setName(string)`

```solidity
///  @notice Update the name of the token
///  @param newName The new name for the token
function _setName(string memory newName) internal {
    _name = newName;
}
```

### _setSymbol(string)

- **Kind**: internal
- **Source**: 7635:84:35
- **Link**: `lib/aave-v3-origin/src/contracts/protocol/tokenization/base/IncentivizedERC20.sol:IncentivizedERC20:_setSymbol(string)`

```solidity
///  @notice Update the symbol for the token
///  @param newSymbol The new symbol for the token
function _setSymbol(string memory newSymbol) internal {
    _symbol = newSymbol;
}
```

### _setDecimals(uint8)

- **Kind**: internal
- **Source**: 7857:84:35
- **Link**: `lib/aave-v3-origin/src/contracts/protocol/tokenization/base/IncentivizedERC20.sol:IncentivizedERC20:_setDecimals(uint8)`

```solidity
///  @notice Update the number of decimals for the token
///  @param newDecimals The new number of decimals for the token
function _setDecimals(uint8 newDecimals) internal {
    _decimals = newDecimals;
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

### initializer()

- **Kind**: modifier
- **Source**: 1131:430:24
- **Link**: `lib/aave-v3-origin/src/contracts/misc/aave-upgradeability/VersionedInitializable.sol:VersionedInitializable:initializer()`

```solidity
///  @dev Modifier to use in the initializer function of a contract.
modifier initializer() {
    uint256 revision = getRevision();
    require((initializing || isConstructor()) || (revision > lastInitializedRevision), "Contract instance has already been initialized");
    bool isTopLevelCall = !initializing;
    if (isTopLevelCall) {
        initializing = true;
        lastInitializedRevision = revision;
    }
    _;
    if (isTopLevelCall) {
        initializing = false;
    }
}
```

### getRevision()

- **Kind**: internal
- **Source**: 443:109:11
- **Link**: `lib/aave-v3-origin/src/contracts/instances/VariableDebtTokenInstance.sol:VariableDebtTokenInstance:getRevision()`

```solidity
/// @inheritdoc VersionedInitializable
function getRevision() virtual override internal pure returns (uint256) {
    return DEBT_TOKEN_REVISION;
}
```

### isConstructor()

- **Kind**: internal
- **Source**: 1962:510:24
- **Link**: `lib/aave-v3-origin/src/contracts/misc/aave-upgradeability/VersionedInitializable.sol:VersionedInitializable:isConstructor()`

```solidity
///  @notice Returns true if and only if the function is running in the constructor
///  @return True if the function is running in the constructor
function isConstructor() private view returns (bool) {
    uint256 cs;
    assembly {
        cs := extcodesize(address())
    }
    return cs == 0;
}
```

## State Variable Reads

- **EIP712_DOMAIN** (`bytes32`)
- **EIP712_REVISION** (`bytes`)
- **_name** (`string`)
- **initializing** (`bool`)
- **lastInitializedRevision** (`uint256`)
- **DEBT_TOKEN_REVISION** (`uint256`)

## State Variable Writes

- **_name** (`string`)
- **_symbol** (`string`)
- **_decimals** (`uint8`)
- **initializing** (`bool`)
- **lastInitializedRevision** (`uint256`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: VariableDebtTokenInstance.initialize(contract IPool,address,contract IAaveIncentivesController,uint8,string,string,bytes) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: external
  ├─ [1] ⚙️ FUNCTION: IncentivizedERC20._setName(string) (NodeID: 1)
  │   💬 Args: [debtTokenName]
  │   👁️  Def: internal
  ├─ [1] ⚙️ FUNCTION: IncentivizedERC20._setSymbol(string) (NodeID: 2)
  │   💬 Args: [debtTokenSymbol]
  │   👁️  Def: internal
  ├─ [1] ⚙️ FUNCTION: IncentivizedERC20._setDecimals(uint8) (NodeID: 3)
  │   💬 Args: [debtTokenDecimals]
  │   👁️  Def: internal
  ├─ [1] ⚙️ FUNCTION: EIP712Base._calculateDomainSeparator() (NodeID: 4)
  │   💬 Args: [no args]
  │   👁️  Def: internal
  │ └─ [2] ⚙️ FUNCTION: VariableDebtToken._EIP712BaseId() (NodeID: 5)
  │     💬 Args: [no args]
  │     👁️  Def: internal
  │   └─ [3] ⚙️ FUNCTION: IncentivizedERC20.name() (NodeID: 6)
  │       💬 Args: [no args]
  │       👁️  Def: public
  └─ [1] 🔒 MODIFIER: VersionedInitializable.initializer() (NodeID: 7)
      💬 Args: [no args]
    ├─ [2] ⚙️ FUNCTION: VariableDebtTokenInstance.getRevision() (NodeID: 8)
    │   💬 Args: [no args]
    │   👁️  Def: internal
    └─ [2] ⚙️ FUNCTION: VersionedInitializable.isConstructor() (NodeID: 9)
        💬 Args: [no args]
        👁️  Def: private
```

## Documentation

### Function Documentation

@inheritdoc IInitializableDebtToken

### Interface Documentation

 @notice Initializes the debt token.
 @param pool The pool contract that is initializing this contract
 @param underlyingAsset The address of the underlying asset of this aToken (E.g. WETH for aWETH)
 @param incentivesController The smart contract managing potential incentives distribution
 @param debtTokenDecimals The decimals of the debtToken, same as the underlying asset's
 @param debtTokenName The name of the token
 @param debtTokenSymbol The symbol of the token
 @param params A set of encoded parameters for additional initialization
