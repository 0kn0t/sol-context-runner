# Contract: InitializableUpgradeabilityProxy

## Metadata

- **Name**: InitializableUpgradeabilityProxy
- **Type**: Contract
- **Path**: lib/aave-v3-origin/src/contracts/dependencies/openzeppelin/upgradeability/InitializableUpgradeabilityProxy.sol
- **Documentation**:  @title InitializableUpgradeabilityProxy
   @dev Extends BaseUpgradeabilityProxy with an initializer for initializing
   implementation and init data.

## State Variables

### IMPLEMENTATION_SLOT (inherited from BaseUpgradeabilityProxy)

```solidity
///  @dev Storage slot with the address of the current implementation.
///  This is the keccak-256 hash of "eip1967.proxy.implementation" subtracted by 1, and is
///  validated in the constructor.
bytes32 internal constant IMPLEMENTATION_SLOT = 0x360894a13ba1a3210667c828492db98dca3e2076cc3735a920a3ca505d382bbc
```

## Events

### Upgraded (inherited from BaseUpgradeabilityProxy)

```solidity
///  @dev Emitted when the implementation is upgraded.
///  @param implementation Address of the new implementation.
event Upgraded(address indexed implementation);
```

## Public/External Functions

### initialize(address,bytes)

- **Signature**: `initialize(address,bytes)`
- **Visibility**: public
- **Source Range**: 855:365:8
- **Details**: [function_initialize_address_bytes.md](./function_initialize_address_bytes.md)

**Signature:**
```solidity
///  @dev Contract initializer.
///  @param _logic Address of the initial implementation.
///  @param _data Data to send as msg.data to the implementation to initialize the proxied contract.
///  It should include the signature and the parameters of the function to be called, as described in
///  https://solidity.readthedocs.io/en/v0.4.24/abi-spec.html#function-selector-and-argument-encoding.
///  This parameter is optional, if no data is given the initialization call to proxied contract will be skipped.
function initialize(address _logic, bytes memory _data) public payable;
```

### fallback() (inherited from Proxy)

- **Signature**: `fallback()`
- **Visibility**: external
- **Source Range**: 534:50:9
- **Details**: [function_fallback.md](./function_fallback.md)

**Signature:**
```solidity
///  @dev Fallback function.
///  Will run if no other function in the contract matches the call data.
///  Implemented entirely in `_fallback`.
fallback() external payable;
```

### receive() (inherited from Proxy)

- **Signature**: `receive()`
- **Visibility**: external
- **Source Range**: 739:49:9
- **Details**: [function_receive.md](./function_receive.md)

**Signature:**
```solidity
///  @dev Fallback function that will run if call data is empty.
///  IMPORTANT. receive() on implementation contracts will be unreachable
receive() external payable;
```
