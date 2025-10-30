# Function: deploy(contract Auth,contract IERC20Metadata,uint256,contract IVault[])

**Contract**: [script/CryticVeryLiquidVaultMock.s.sol/contract_CryticVeryLiquidVaultMockScript.md]

## Metadata

- **Contract**: CryticVeryLiquidVaultMockScript
- **Signature**: `deploy(contract Auth,contract IERC20Metadata,uint256,contract IVault[])`
- **Visibility**: public
- **Source Range**: 1620:1236:132

## Implementation

```solidity
function deploy(Auth auth_, IERC20Metadata asset_, uint256 firstDepositAmount_, IVault[] memory strategies_) public returns (CryticVeryLiquidVaultMock cryticVeryLiquidVaultMock) {
    string memory name = string.concat("Very Liquid Crytic ", asset_.name(), " Vault");
    string memory symbol = string.concat("vlv", "Crytic", asset_.symbol(), "Mock");
    address implementation = address(new CryticVeryLiquidVaultMock());
    bytes memory initializationData = abi.encodeCall(VeryLiquidVault.initialize, (auth_, asset_, name, symbol, fundingAccount, firstDepositAmount_, strategies_));
    bytes memory creationCode = abi.encodePacked(type(ERC1967Proxy).creationCode, abi.encode(implementation, initializationData));
    bytes32 salt = keccak256(initializationData);
    cryticVeryLiquidVaultMock = CryticVeryLiquidVaultMock(create2Deployer.computeAddress(salt, keccak256(creationCode)));
    asset_.forceApprove(address(cryticVeryLiquidVaultMock), firstDepositAmount_);
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
┌─ [0] ⚙️ FUNCTION: CryticVeryLiquidVaultMockScript.deploy(contract Auth,contract IERC20Metadata,uint256,contract IVault[]) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
```
