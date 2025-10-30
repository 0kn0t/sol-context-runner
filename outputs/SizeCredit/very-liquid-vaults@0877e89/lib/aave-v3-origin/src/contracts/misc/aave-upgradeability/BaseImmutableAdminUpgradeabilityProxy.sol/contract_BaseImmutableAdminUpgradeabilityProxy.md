# Contract: BaseImmutableAdminUpgradeabilityProxy

## Metadata

- **Name**: BaseImmutableAdminUpgradeabilityProxy
- **Type**: Contract
- **Path**: lib/aave-v3-origin/src/contracts/misc/aave-upgradeability/BaseImmutableAdminUpgradeabilityProxy.sol
- **Documentation**:  @title BaseImmutableAdminUpgradeabilityProxy
   @author Aave, inspired by the OpenZeppelin upgradeability proxy pattern
   @notice This contract combines an upgradeability proxy with an authorization
   mechanism for administrative tasks.
   @dev The admin role is stored in an immutable, which helps saving transactions costs
   All external functions in this contract must be guarded by the
   `ifAdmin` modifier. See ethereum/solidity#3864 for a Solidity
   feature proposal that would enable this to be done automatically.

## State Variables

### IMPLEMENTATION_SLOT (inherited from BaseUpgradeabilityProxy)

```solidity
///  @dev Storage slot with the address of the current implementation.
///  This is the keccak-256 hash of "eip1967.proxy.implementation" subtracted by 1, and is
///  validated in the constructor.
bytes32 internal constant IMPLEMENTATION_SLOT = 0x360894a13ba1a3210667c828492db98dca3e2076cc3735a920a3ca505d382bbc
```

### _admin

```solidity
address internal immutable _admin
```

## Events

### Upgraded (inherited from BaseUpgradeabilityProxy)

```solidity
///  @dev Emitted when the implementation is upgraded.
///  @param implementation Address of the new implementation.
event Upgraded(address indexed implementation);
```

## Public/External Functions

### constructor(address)

- **Signature**: `constructor(address)`
- **Visibility**: public
- **Source Range**: 908:54:22
- **Details**: [function_constructor_address.md](./function_constructor_address.md)

**Signature:**
```solidity
///  @dev Constructor.
///  @param admin_ The address of the admin
constructor(address admin_);
```

### admin()

- **Signature**: `admin()`
- **Visibility**: external
- **Source Range**: 1168:76:22
- **Details**: [function_admin.md](./function_admin.md)

**Signature:**
```solidity
///  @notice Return the admin address
///  @return The address of the proxy admin.
function admin() external ifAdmin() returns (address);
```

### implementation()

- **Signature**: `implementation()`
- **Visibility**: external
- **Source Range**: 1355:96:22
- **Details**: [function_implementation.md](./function_implementation.md)

**Signature:**
```solidity
///  @notice Return the implementation address
///  @return The address of the implementation.
function implementation() external ifAdmin() returns (address);
```

### upgradeTo(address)

- **Signature**: `upgradeTo(address)`
- **Visibility**: external
- **Source Range**: 1647:103:22
- **Details**: [function_upgradeTo_address.md](./function_upgradeTo_address.md)

**Signature:**
```solidity
///  @notice Upgrade the backing implementation of the proxy.
///  @dev Only the admin can call this function.
///  @param newImplementation The address of the new implementation.
function upgradeTo(address newImplementation) external ifAdmin();
```

### upgradeToAndCall(address,bytes)

- **Signature**: `upgradeToAndCall(address,bytes)`
- **Visibility**: external
- **Source Range**: 2279:234:22
- **Details**: [function_upgradeToAndCall_address_bytes.md](./function_upgradeToAndCall_address_bytes.md)

**Signature:**
```solidity
///  @notice Upgrade the backing implementation of the proxy and call a function
///  on the new implementation.
///  @dev This is useful to initialize the proxied contract.
///  @param newImplementation The address of the new implementation.
///  @param data Data to send as msg.data in the low level call.
///  It should include the signature and the parameters of the function to be called, as described in
///  https://solidity.readthedocs.io/en/v0.4.24/abi-spec.html#function-selector-and-argument-encoding.
function upgradeToAndCall(address newImplementation, bytes calldata data) external payable ifAdmin();
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
