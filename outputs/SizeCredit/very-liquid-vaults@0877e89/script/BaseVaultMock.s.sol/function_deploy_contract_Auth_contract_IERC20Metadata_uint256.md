# Function: deploy(contract Auth,contract IERC20Metadata,uint256)

**Contract**: [script/BaseVaultMock.s.sol/contract_BaseVaultMockScript.md]

## Metadata

- **Contract**: BaseVaultMockScript
- **Signature**: `deploy(contract Auth,contract IERC20Metadata,uint256)`
- **Visibility**: public
- **Source Range**: 1245:1095:126

## Implementation

```solidity
function deploy(Auth auth_, IERC20Metadata asset_, uint256 firstDepositAmount_) public returns (BaseVaultMock baseVaultMock) {
    string memory name = string.concat("Very Liquid Base ", asset_.name(), " Mock Vault");
    string memory symbol = string.concat("vlv", "Base", asset_.symbol(), "Mock");
    address implementation = address(new BaseVaultMock());
    bytes memory initializationData = abi.encodeCall(BaseVault.initialize, (auth_, asset_, name, symbol, fundingAccount, firstDepositAmount_));
    bytes memory creationCode = abi.encodePacked(type(ERC1967Proxy).creationCode, abi.encode(implementation, initializationData));
    bytes32 salt = keccak256(initializationData);
    baseVaultMock = BaseVaultMock(create2Deployer.computeAddress(salt, keccak256(creationCode)));
    asset_.forceApprove(address(baseVaultMock), firstDepositAmount_);
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
┌─ [0] ⚙️ FUNCTION: BaseVaultMockScript.deploy(contract Auth,contract IERC20Metadata,uint256) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
```
