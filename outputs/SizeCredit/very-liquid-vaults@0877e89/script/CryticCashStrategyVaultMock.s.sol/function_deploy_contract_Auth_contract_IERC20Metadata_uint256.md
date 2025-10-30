# Function: deploy(contract Auth,contract IERC20Metadata,uint256)

**Contract**: [script/CryticCashStrategyVaultMock.s.sol/contract_CryticCashStrategyVaultMockScript.md]

## Metadata

- **Contract**: CryticCashStrategyVaultMockScript
- **Signature**: `deploy(contract Auth,contract IERC20Metadata,uint256)`
- **Visibility**: public
- **Source Range**: 1295:1207:130

## Implementation

```solidity
function deploy(Auth auth_, IERC20Metadata asset_, uint256 firstDepositAmount_) public returns (CryticCashStrategyVaultMock cryticCashStrategyVaultMock) {
    string memory name = string.concat("Very Liquid Crytic Cash ", asset_.name(), " Strategy Mock Vault");
    string memory symbol = string.concat("vlv", "Cash", asset_.symbol(), "Mock");
    address implementation = address(new CryticCashStrategyVaultMock());
    bytes memory initializationData = abi.encodeCall(BaseVault.initialize, (auth_, asset_, name, symbol, fundingAccount, firstDepositAmount_));
    bytes memory creationCode = abi.encodePacked(type(ERC1967Proxy).creationCode, abi.encode(implementation, initializationData));
    bytes32 salt = keccak256(initializationData);
    cryticCashStrategyVaultMock = CryticCashStrategyVaultMock(create2Deployer.computeAddress(salt, keccak256(creationCode)));
    asset_.forceApprove(address(cryticCashStrategyVaultMock), firstDepositAmount_);
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
┌─ [0] ⚙️ FUNCTION: CryticCashStrategyVaultMockScript.deploy(contract Auth,contract IERC20Metadata,uint256) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
```
