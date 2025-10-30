# Function: deploy(contract Auth,contract IERC20Metadata,uint256,contract IPool)

**Contract**: [script/AaveStrategyVault.s.sol/contract_AaveStrategyVaultScript.md]

## Metadata

- **Contract**: AaveStrategyVaultScript
- **Signature**: `deploy(contract Auth,contract IERC20Metadata,uint256,contract IPool)`
- **Visibility**: public
- **Source Range**: 1331:1153:122

## Implementation

```solidity
function deploy(Auth auth_, IERC20Metadata asset_, uint256 firstDepositAmount_, IPool pool_) public returns (AaveStrategyVault aaveStrategyVault) {
    string memory name = string.concat("Very Liquid Aave ", asset_.name(), " Strategy Vault");
    string memory symbol = string.concat("vlv", "Aave", asset_.symbol());
    address implementation = address(new AaveStrategyVault());
    bytes memory initializationData = abi.encodeCall(AaveStrategyVault.initialize, (auth_, asset_, name, symbol, fundingAccount, firstDepositAmount_, pool_));
    bytes memory creationCode = abi.encodePacked(type(ERC1967Proxy).creationCode, abi.encode(implementation, initializationData));
    bytes32 salt = keccak256(initializationData);
    aaveStrategyVault = AaveStrategyVault(create2Deployer.computeAddress(salt, keccak256(creationCode)));
    asset_.forceApprove(address(aaveStrategyVault), firstDepositAmount_);
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
┌─ [0] ⚙️ FUNCTION: AaveStrategyVaultScript.deploy(contract Auth,contract IERC20Metadata,uint256,contract IPool) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
```
