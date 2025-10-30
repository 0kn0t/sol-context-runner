# Contract: DummyConstraint

## Metadata

- **Name**: DummyConstraint
- **Type**: Contract
- **Path**: src/DummyConstraint.sol
- **Documentation**: @notice Dummy constraint used as a placeholder

## Implements Interfaces

- **IConstraint** [src/Dependencies/IConstraint.sol/interface_IConstraint.md]

## Errors

### ConstraintCheckFailed (inherited from IConstraint)

```solidity
error ConstraintCheckFailed(address constraint, uint256 amount, address bsm, bytes errData);
```

## Events

### ConstraintUpdated (inherited from IConstraint)

```solidity
event ConstraintUpdated(address indexed oldConstraint, address indexed newConstraint);
```

## Public/External Functions

### canProcess(uint256,address)

- **Signature**: `canProcess(uint256,address)`
- **Visibility**: external
- **Source Range**: 252:128:38
- **Details**: [function_canProcess_uint256_address.md](./function_canProcess_uint256_address.md)

**Signature:**
```solidity
/// @notice Returns true
function canProcess(uint256 _amount, address _bsm) external view returns (bool, bytes memory);
```
