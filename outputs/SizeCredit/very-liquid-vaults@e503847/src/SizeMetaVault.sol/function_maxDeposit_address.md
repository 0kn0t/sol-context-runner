# Function: maxDeposit(address)

**Contract**: [src/SizeMetaVault.sol/contract_SizeMetaVault.md]

## Metadata

- **Contract**: SizeMetaVault
- **Signature**: `maxDeposit(address)`
- **Visibility**: public
- **Source Range**: 3979:163:59

## Implementation

```solidity
/// @notice Returns the maximum amount that can be deposited
function maxDeposit(address receiver) override(BaseVault) public view returns (uint256) {
    return Math.min(_maxDeposit(), super.maxDeposit(receiver));
}
```

## Related Implementations

### min(uint256,uint256)

- **Kind**: internal
- **Source**: 5617:111:52
- **Link**: `lib/openzeppelin-contracts/contracts/utils/math/Math.sol:Math:min(uint256,uint256)`

```solidity
///  @dev Returns the smallest of two numbers.
function min(uint256 a, uint256 b) internal pure returns (uint256) {
    return ternary(a < b, a, b);
}
```

### _maxDeposit()

- **Kind**: internal
- **Source**: 15537:355:59
- **Link**: `src/SizeMetaVault.sol:SizeMetaVault:_maxDeposit()`

```solidity
/// @notice Internal function to calculate maximum depositable amount in all strategies
function _maxDeposit() private view returns (uint256 max) {
    uint256 length = strategies.length;
    for (uint256 i = 0; i < length; i++) {
        IBaseVault strategy = strategies[i];
        uint256 strategyMaxDeposit = strategy.maxDeposit(address(this));
        max = Math.saturatingAdd(max, strategyMaxDeposit);
    }
}
```

### saturatingAdd(uint256,uint256)

- **Kind**: internal
- **Source**: 3912:199:52
- **Link**: `lib/openzeppelin-contracts/contracts/utils/math/Math.sol:Math:saturatingAdd(uint256,uint256)`

```solidity
///  @dev Unsigned saturating addition, bounds to `2²⁵⁶ - 1` instead of overflowing.
function saturatingAdd(uint256 a, uint256 b) internal pure returns (uint256) {
    (bool success, uint256 result) = tryAdd(a, b);
    return ternary(success, result, type(uint256).max);
}
```

### tryAdd(uint256,uint256)

- **Kind**: internal
- **Source**: 1693:240:52
- **Link**: `lib/openzeppelin-contracts/contracts/utils/math/Math.sol:Math:tryAdd(uint256,uint256)`

```solidity
///  @dev Returns the addition of two unsigned integers, with a success flag (no overflow).
function tryAdd(uint256 a, uint256 b) internal pure returns (bool success, uint256 result) {
    unchecked {
        uint256 c = a + b;
        success = c >= a;
        result = c * SafeCast.toUint(success);
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

### maxDeposit(address)

- **Kind**: internal
- **Source**: 8184:249:56
- **Link**: `src/BaseVault.sol:BaseVault:maxDeposit(address)`

```solidity
/// @notice Returns the maximum amount that can be deposited
///  @dev Returns type(uint256).max if no total assets cap is set
function maxDeposit(address) virtual override(ERC4626Upgradeable, IERC4626) public view returns (uint256) {
    return (totalAssetsCap == type(uint256).max) ? type(uint256).max : Math.saturatingSub(totalAssetsCap, totalAssets());
}
```

### saturatingSub(uint256,uint256)

- **Kind**: internal
- **Source**: 4217:150:52
- **Link**: `lib/openzeppelin-contracts/contracts/utils/math/Math.sol:Math:saturatingSub(uint256,uint256)`

```solidity
///  @dev Unsigned saturating subtraction, bounds to zero instead of overflowing.
function saturatingSub(uint256 a, uint256 b) internal pure returns (uint256) {
    (, uint256 result) = trySub(a, b);
    return result;
}
```

### totalAssets()

- **Kind**: internal
- **Source**: 5471:264:59
- **Link**: `src/SizeMetaVault.sol:SizeMetaVault:totalAssets()`

```solidity
/// @notice Returns the total assets managed by the vault
function totalAssets() virtual override(ERC4626Upgradeable, IERC4626) public view returns (uint256 total) {
    uint256 length = strategies.length;
    for (uint256 i = 0; i < length; i++) {
        total += strategies[i].totalAssets();
    }
}
```

### trySub(uint256,uint256)

- **Kind**: internal
- **Source**: 2052:240:52
- **Link**: `lib/openzeppelin-contracts/contracts/utils/math/Math.sol:Math:trySub(uint256,uint256)`

```solidity
///  @dev Returns the subtraction of two unsigned integers, with a success flag (no overflow).
function trySub(uint256 a, uint256 b) internal pure returns (bool success, uint256 result) {
    unchecked {
        uint256 c = a - b;
        success = c <= a;
        result = c * SafeCast.toUint(success);
    }
}
```

## State Variable Reads

- **strategies** (`contract IBaseVault[]`) [src/IBaseVault.sol/interface_IBaseVault.md]
- **totalAssetsCap** (`uint256`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: SizeMetaVault.maxDeposit(address) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
  └─ [1] ⚙️ FUNCTION: Math.min(uint256,uint256) (NodeID: 1)
      💬 Args: [_maxDeposit(), super.maxDeposit(receiver)]
      👁️  Def: internal
    ├─ [2] ⚙️ FUNCTION: SizeMetaVault._maxDeposit() (NodeID: 4)
    │   💬 Args: [no args]
    │   👁️  Def: private
    │ └─ [3] ⚙️ FUNCTION: Math.saturatingAdd(uint256,uint256) (NodeID: 5)
    │     💬 Args: [max, strategyMaxDeposit]
    │     👁️  Def: internal
    │   ├─ [4] ⚙️ FUNCTION: Math.tryAdd(uint256,uint256) (NodeID: 6)
    │   │   💬 Args: [a, b]
    │   │   👁️  Def: internal
    │   │ └─ [5] ⚙️ FUNCTION: SafeCast.toUint(bool) (NodeID: 7)
    │   │     💬 Args: [success]
    │   │     👁️  Def: internal
    │   └─ [4] ⚙️ FUNCTION: Math.ternary(bool,uint256,uint256) (NodeID: 8)
    │       💬 Args: [success, result, type(uint256).max]
    │       👁️  Def: internal
    │     └─ [5] ⚙️ FUNCTION: SafeCast.toUint(bool) (NodeID: 9)
    │         💬 Args: [condition]
    │         👁️  Def: internal
    ├─ [2] ⚙️ FUNCTION: BaseVault.maxDeposit(address) (NodeID: 10)
    │   💬 Args: [receiver]
    │   👁️  Def: public
    │ └─ [3] ⚙️ FUNCTION: Math.saturatingSub(uint256,uint256) (NodeID: 11)
    │     💬 Args: [totalAssetsCap, totalAssets()]
    │     👁️  Def: internal
    │   ├─ [4] ⚙️ FUNCTION: SizeMetaVault.totalAssets() (NodeID: 14)
    │   │   💬 Args: [no args]
    │   │   👁️  Def: public
    │   └─ [4] ⚙️ FUNCTION: Math.trySub(uint256,uint256) (NodeID: 12)
    │       💬 Args: [a, b]
    │       👁️  Def: internal
    │     └─ [5] ⚙️ FUNCTION: SafeCast.toUint(bool) (NodeID: 13)
    │         💬 Args: [success]
    │         👁️  Def: internal
    └─ [2] ⚙️ FUNCTION: Math.ternary(bool,uint256,uint256) (NodeID: 2)
        💬 Args: [a < b, a, b]
        👁️  Def: internal
      └─ [3] ⚙️ FUNCTION: SafeCast.toUint(bool) (NodeID: 3)
          💬 Args: [condition]
          👁️  Def: internal
```

## Documentation

### Function Documentation

@notice Returns the maximum amount that can be deposited

### Interface Documentation

 @dev Returns the maximum amount of the underlying asset that can be deposited into the Vault for the receiver,
 through a deposit call.
 - MUST return a limited value if receiver is subject to some deposit limit.
 - MUST return 2 ** 256 - 1 if there is no limit on the maximum amount of assets that may be deposited.
 - MUST NOT revert.
