# Interface: IHevm

## Metadata

- **Name**: IHevm
- **Type**: Interface
- **Path**: lib/properties/contracts/util/Hevm.sol

## Public/External Functions

### warp(uint256)

- **Signature**: `warp(uint256)`
- **Visibility**: external
- **Source Range**: 128:45:114

**Signature:**
```solidity
function warp(uint256 newTimestamp) external;;
```

### roll(uint256)

- **Signature**: `roll(uint256)`
- **Visibility**: external
- **Source Range**: 216:42:114

**Signature:**
```solidity
function roll(uint256 newNumber) external;;
```

### assume(bool)

- **Signature**: `assume(bool)`
- **Visibility**: external
- **Source Range**: 389:33:114

**Signature:**
```solidity
function assume(bool b) external;;
```

### deal(address,uint256)

- **Signature**: `deal(address,uint256)`
- **Visibility**: external
- **Source Range**: 470:49:114

**Signature:**
```solidity
function deal(address usr, uint256 amt) external;;
```

### load(address,bytes32)

- **Signature**: `load(address,bytes32)`
- **Visibility**: external
- **Source Range**: 569:70:114

**Signature:**
```solidity
function load(address where, bytes32 slot) external returns (bytes32);;
```

### store(address,bytes32,bytes32)

- **Signature**: `store(address,bytes32,bytes32)`
- **Visibility**: external
- **Source Range**: 695:68:114

**Signature:**
```solidity
function store(address where, bytes32 slot, bytes32 value) external;;
```

### sign(uint256,bytes32)

- **Signature**: `sign(uint256,bytes32)`
- **Visibility**: external
- **Source Range**: 821:121:114

**Signature:**
```solidity
function sign(uint256 privateKey, bytes32 digest) external returns (uint8 v, bytes32 r, bytes32 s);;
```

### addr(uint256)

- **Signature**: `addr(uint256)`
- **Visibility**: external
- **Source Range**: 992:66:114

**Signature:**
```solidity
function addr(uint256 privateKey) external returns (address addr);;
```

### ffi(string[])

- **Signature**: `ffi(string[])`
- **Visibility**: external
- **Source Range**: 1117:92:114

**Signature:**
```solidity
function ffi(string[] calldata inputs) external returns (bytes memory result);;
```

### prank(address)

- **Signature**: `prank(address)`
- **Visibility**: external
- **Source Range**: 1288:43:114

**Signature:**
```solidity
function prank(address newSender) external;;
```

### createFork(string)

- **Signature**: `createFork(string)`
- **Visibility**: external
- **Source Range**: 1447:75:114

**Signature:**
```solidity
function createFork(string calldata urlOrAlias) external returns (uint256);;
```

### selectFork(uint256)

- **Signature**: `selectFork(uint256)`
- **Visibility**: external
- **Source Range**: 1631:45:114

**Signature:**
```solidity
function selectFork(uint256 forkId) external;;
```

### activeFork()

- **Signature**: `activeFork()`
- **Visibility**: external
- **Source Range**: 1732:49:114

**Signature:**
```solidity
function activeFork() external returns (uint256);;
```

### label(address,string)

- **Signature**: `label(address,string)`
- **Visibility**: external
- **Source Range**: 1823:61:114

**Signature:**
```solidity
function label(address addr, string calldata label) external;;
```
