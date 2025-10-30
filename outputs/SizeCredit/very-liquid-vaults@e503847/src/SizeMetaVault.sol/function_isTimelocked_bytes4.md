# Function: isTimelocked(bytes4)

**Contract**: [src/SizeMetaVault.sol/contract_SizeMetaVault.md]

## Metadata

- **Contract**: SizeMetaVault
- **Signature**: `isTimelocked(bytes4)`
- **Visibility**: public
- **Source Range**: 1945:166:64
- **Inherited From**: Timelock

## Implementation

```solidity
/// @notice Checks if the function is timelocked
///  @return True if the function is timelocked, false otherwise
function isTimelocked(bytes4 sig) public view returns (bool) {
    return block.timestamp < (_getTimelockDuration(sig) + timelockData[sig].proposedTimestamp);
}
```

## Related Implementations

### _getTimelockDuration(bytes4)

- **Kind**: internal
- **Source**: 4424:161:64
- **Link**: `src/utils/Timelock.sol:Timelock:_getTimelockDuration(bytes4)`

```solidity
/// @notice Gets the timelock duration for a specific function
function _getTimelockDuration(bytes4 sig) internal view returns (uint256) {
    return Math.max(MINIMUM_TIMELOCK_DURATION, timelockData[sig].duration);
}
```

### max(uint256,uint256)

- **Kind**: internal
- **Source**: 5435:111:52
- **Link**: `lib/openzeppelin-contracts/contracts/utils/math/Math.sol:Math:max(uint256,uint256)`

```solidity
///  @dev Returns the largest of two numbers.
function max(uint256 a, uint256 b) internal pure returns (uint256) {
    return ternary(a > b, a, b);
}
```

### ternary(bool,uint256,uint256)

- **Kind**: internal
- **Source**: 5071:294:52
- **Link**: `lib/openzeppelin-contracts/contracts/utils/math/Math.sol:Math:ternary(bool,uint256,uint256)`

```solidity
///  @dev Branchless ternary evaluation for `a ? b : c`. Gas costs are constant.
///  IMPORTANT: This function may reduce bytecode size and consume less gas when used standalone.
///  However, the compiler may optimize Solidity ternary operations (i.e. `a ? b : c`) to only compute
///  one branch when needed, making this function more expensive.
function ternary(bool condition, uint256 a, uint256 b) internal pure returns (uint256) {
    unchecked {
        return b ^ ((a ^ b) * SafeCast.toUint(condition));
    }
}
```

### toUint(bool)

- **Kind**: internal
- **Source**: 34795:145:53
- **Link**: `lib/openzeppelin-contracts/contracts/utils/math/SafeCast.sol:SafeCast:toUint(bool)`

```solidity
///  @dev Cast a boolean (false or true) to a uint256 (0 or 1) with no jump.
function toUint(bool b) internal pure returns (uint256 u) {
    assembly ("memory-safe") {
        u := iszero(iszero(b))
    }
}
```

## State Variable Reads

- **timelockData** (`mapping(bytes4 => struct Timelock.TimelockData)`)
- **MINIMUM_TIMELOCK_DURATION** (`uint256`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: Timelock.isTimelocked(bytes4) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
  └─ [1] ⚙️ FUNCTION: Timelock._getTimelockDuration(bytes4) (NodeID: 1)
      💬 Args: [sig]
      👁️  Def: internal
    └─ [2] ⚙️ FUNCTION: Math.max(uint256,uint256) (NodeID: 2)
        💬 Args: [MINIMUM_TIMELOCK_DURATION, timelockData[sig].duration]
        👁️  Def: internal
      └─ [3] ⚙️ FUNCTION: Math.ternary(bool,uint256,uint256) (NodeID: 3)
          💬 Args: [a > b, a, b]
          👁️  Def: internal
        └─ [4] ⚙️ FUNCTION: SafeCast.toUint(bool) (NodeID: 4)
            💬 Args: [condition]
            👁️  Def: internal
```

## Documentation

### Function Documentation

@notice Checks if the function is timelocked
 @return True if the function is timelocked, false otherwise
