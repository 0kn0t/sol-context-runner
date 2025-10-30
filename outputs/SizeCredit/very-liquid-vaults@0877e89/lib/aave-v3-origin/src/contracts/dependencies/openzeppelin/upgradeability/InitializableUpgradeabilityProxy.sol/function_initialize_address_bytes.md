# Function: initialize(address,bytes)

**Contract**: [lib/aave-v3-origin/src/contracts/dependencies/openzeppelin/upgradeability/InitializableUpgradeabilityProxy.sol/contract_InitializableUpgradeabilityProxy.md]

## Metadata

- **Contract**: InitializableUpgradeabilityProxy
- **Signature**: `initialize(address,bytes)`
- **Visibility**: public
- **Source Range**: 855:365:8

## Implementation

```solidity
///  @dev Contract initializer.
///  @param _logic Address of the initial implementation.
///  @param _data Data to send as msg.data to the implementation to initialize the proxied contract.
///  It should include the signature and the parameters of the function to be called, as described in
///  https://solidity.readthedocs.io/en/v0.4.24/abi-spec.html#function-selector-and-argument-encoding.
///  This parameter is optional, if no data is given the initialization call to proxied contract will be skipped.
function initialize(address _logic, bytes memory _data) public payable {
    require(_implementation() == address(0));
    assert(IMPLEMENTATION_SLOT == bytes32(uint256(keccak256("eip1967.proxy.implementation")) - 1));
    _setImplementation(_logic);
    if (_data.length > 0) {
        (bool success, ) = _logic.delegatecall(_data);
        require(success);
    }
}
```

## Related Implementations

### _implementation()

- **Kind**: internal
- **Source**: 1004:196:7
- **Link**: `lib/aave-v3-origin/src/contracts/dependencies/openzeppelin/upgradeability/BaseUpgradeabilityProxy.sol:BaseUpgradeabilityProxy:_implementation()`

```solidity
///  @dev Returns the current implementation.
///  @return impl Address of the current implementation
function _implementation() override internal view returns (address impl) {
    bytes32 slot = IMPLEMENTATION_SLOT;
    assembly {
        impl := sload(slot)
    }
}
```

### _setImplementation(address)

- **Kind**: internal
- **Source**: 1614:334:7
- **Link**: `lib/aave-v3-origin/src/contracts/dependencies/openzeppelin/upgradeability/BaseUpgradeabilityProxy.sol:BaseUpgradeabilityProxy:_setImplementation(address)`

```solidity
///  @dev Sets the implementation address of the proxy.
///  @param newImplementation Address of the new implementation.
function _setImplementation(address newImplementation) internal {
    require(Address.isContract(newImplementation), "Cannot set a proxy implementation to a non-contract address");
    bytes32 slot = IMPLEMENTATION_SLOT;
    assembly {
        sstore(slot, newImplementation)
    }
}
```

### isContract(address)

- **Kind**: internal
- **Source**: 735:341:1
- **Link**: `lib/aave-v3-origin/src/contracts/dependencies/openzeppelin/contracts/Address.sol:Address:isContract(address)`

```solidity
///  @dev Returns true if `account` is a contract.
///  [IMPORTANT]
///  ====
///  It is unsafe to assume that an address for which this function returns
///  false is an externally-owned account (EOA) and not a contract.
///  Among others, `isContract` will return false for the following
///  types of addresses:
///   - an externally-owned account
///   - a contract in construction
///   - an address where a contract will be created
///   - an address where a contract lived, but was destroyed
///  ====
function isContract(address account) internal view returns (bool) {
    uint256 size;
    assembly {
        size := extcodesize(account)
    }
    return size > 0;
}
```

## External Calls

- **address::delegatecall(bytes memory)**

## State Variable Reads

- **IMPLEMENTATION_SLOT** (`bytes32`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: InitializableUpgradeabilityProxy.initialize(address,bytes) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
  ├─ [1] ⚙️ FUNCTION: BaseUpgradeabilityProxy._implementation() (NodeID: 1)
  │   💬 Args: [no args]
  │   👁️  Def: internal
  └─ [1] ⚙️ FUNCTION: BaseUpgradeabilityProxy._setImplementation(address) (NodeID: 2)
      💬 Args: [_logic]
      👁️  Def: internal
    └─ [2] ⚙️ FUNCTION: Address.isContract(address) (NodeID: 3)
        💬 Args: [newImplementation]
        👁️  Def: internal
```

## Documentation

### Function Documentation

 @dev Contract initializer.
 @param _logic Address of the initial implementation.
 @param _data Data to send as msg.data to the implementation to initialize the proxied contract.
 It should include the signature and the parameters of the function to be called, as described in
 https://solidity.readthedocs.io/en/v0.4.24/abi-spec.html#function-selector-and-argument-encoding.
 This parameter is optional, if no data is given the initialization call to proxied contract will be skipped.
