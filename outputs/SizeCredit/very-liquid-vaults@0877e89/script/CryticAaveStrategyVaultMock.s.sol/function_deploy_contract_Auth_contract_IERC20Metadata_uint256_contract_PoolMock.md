# Function: deploy(contract Auth,contract IERC20Metadata,uint256,contract PoolMock)

**Contract**: [script/CryticAaveStrategyVaultMock.s.sol/contract_CryticAaveStrategyVaultMockScript.md]

## Metadata

- **Contract**: CryticAaveStrategyVaultMockScript
- **Signature**: `deploy(contract Auth,contract IERC20Metadata,uint256,contract PoolMock)`
- **Visibility**: public
- **Source Range**: 1442:1248:129

## Implementation

```solidity
function deploy(Auth auth_, IERC20Metadata asset_, uint256 firstDepositAmount_, PoolMock pool_) public returns (CryticAaveStrategyVaultMock cryticAaveStrategyVaultMock) {
    string memory name = string.concat("Very Liquid Crytic Aave ", asset_.name(), " Strategy Mock Vault");
    string memory symbol = string.concat("vlv", "Aave", asset_.symbol(), "Mock");
    address implementation = address(new CryticAaveStrategyVaultMock());
    bytes memory initializationData = abi.encodeCall(AaveStrategyVault.initialize, (auth_, asset_, name, symbol, fundingAccount, firstDepositAmount_, pool_));
    bytes memory creationCode = abi.encodePacked(type(ERC1967Proxy).creationCode, abi.encode(implementation, initializationData));
    bytes32 salt = keccak256(initializationData);
    cryticAaveStrategyVaultMock = CryticAaveStrategyVaultMock(create2Deployer.computeAddress(salt, keccak256(creationCode)));
    asset_.forceApprove(address(cryticAaveStrategyVaultMock), firstDepositAmount_);
    create2Deployer.deploy(0, salt, abi.encodePacked(type(ERC1967Proxy).creationCode, abi.encode(implementation, initializationData)));
}
```

## External Calls

- **IERC20Metadata::name()**
- **IERC20Metadata::symbol()**
- **ICreate2Deployer::computeAddress(bytes32,bytes32)**
- **IERC20Metadata::forceApprove(contract IERC20,address,uint256)**
- **ICreate2Deployer::deploy(uint256,bytes32,bytes)**

## State Variable Reads

- **fundingAccount** (`address`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: CryticAaveStrategyVaultMockScript.deploy(contract Auth,contract IERC20Metadata,uint256,contract PoolMock) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
```
