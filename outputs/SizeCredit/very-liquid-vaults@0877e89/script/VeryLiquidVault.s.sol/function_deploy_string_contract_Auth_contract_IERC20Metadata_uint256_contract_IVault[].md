# Function: deploy(string,contract Auth,contract IERC20Metadata,uint256,contract IVault[])

**Contract**: [script/VeryLiquidVault.s.sol/contract_VeryLiquidVaultScript.md]

## Metadata

- **Contract**: VeryLiquidVaultScript
- **Signature**: `deploy(string,contract Auth,contract IERC20Metadata,uint256,contract IVault[])`
- **Visibility**: public
- **Source Range**: 1745:1297:142

## Implementation

```solidity
function deploy(string memory identifier_, Auth auth_, IERC20Metadata asset_, uint256 veryLiquidVaultFirstDepositAmount_, IVault[] memory strategies_) public returns (VeryLiquidVault veryLiquidVault) {
    string memory name = string.concat("Very Liquid ", identifier_, " ", asset_.name(), " Vault");
    string memory symbol = string.concat("vlv", identifier_, asset_.symbol());
    address implementation = address(new VeryLiquidVault());
    bytes memory initializationData = abi.encodeCall(VeryLiquidVault.initialize, (auth_, asset_, name, symbol, fundingAccount, veryLiquidVaultFirstDepositAmount_, strategies_));
    bytes memory creationCode = abi.encodePacked(type(ERC1967Proxy).creationCode, abi.encode(implementation, initializationData));
    bytes32 salt = keccak256(initializationData);
    veryLiquidVault = VeryLiquidVault(create2Deployer.computeAddress(salt, keccak256(creationCode)));
    IERC20(address(asset_)).forceApprove(address(veryLiquidVault), veryLiquidVaultFirstDepositAmount_);
    create2Deployer.deploy(0, salt, abi.encodePacked(type(ERC1967Proxy).creationCode, abi.encode(implementation, initializationData)));
}
```

## External Calls

- **IERC20Metadata::name()**
- **IERC20Metadata::symbol()**
- **ICreate2Deployer::computeAddress(bytes32,bytes32)**
- **IERC20::forceApprove(contract IERC20,address,uint256)**
- **ICreate2Deployer::deploy(uint256,bytes32,bytes)**

## State Variable Reads

- **fundingAccount** (`address`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: VeryLiquidVaultScript.deploy(string,contract Auth,contract IERC20Metadata,uint256,contract IVault[]) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
```
