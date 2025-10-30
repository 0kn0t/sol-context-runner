# Interface: ICreate2Deployer

## Metadata

- **Name**: ICreate2Deployer
- **Type**: Interface
- **Path**: script/ICreate2Deployer.s.sol

## Public/External Functions

### deploy(uint256,bytes32,bytes)

- **Signature**: `deploy(uint256,bytes32,bytes)`
- **Visibility**: external
- **Source Range**: 90:73:134

**Signature:**
```solidity
function deploy(uint256 value, bytes32 salt, bytes memory code) external;;
```

### computeAddress(bytes32,bytes32)

- **Signature**: `computeAddress(bytes32,bytes32)`
- **Visibility**: external
- **Source Range**: 168:88:134

**Signature:**
```solidity
function computeAddress(bytes32 salt, bytes32 codeHash) external view returns (address);;
```
