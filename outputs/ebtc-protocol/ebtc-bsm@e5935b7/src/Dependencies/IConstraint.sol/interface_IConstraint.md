# Interface: IConstraint

## Metadata

- **Name**: IConstraint
- **Type**: Interface
- **Path**: src/Dependencies/IConstraint.sol

## Errors

### ConstraintCheckFailed

```solidity
error ConstraintCheckFailed(address constraint, uint256 amount, address bsm, bytes errData);
```

## Events

### ConstraintUpdated

```solidity
event ConstraintUpdated(address indexed oldConstraint, address indexed newConstraint);
```

## Public/External Functions

### canProcess(uint256,address)

- **Signature**: `canProcess(uint256,address)`
- **Visibility**: external
- **Source Range**: 285:94:30

**Signature:**
```solidity
function canProcess(uint256 _amount, address _bsm) external view returns (bool, bytes memory);;
```
