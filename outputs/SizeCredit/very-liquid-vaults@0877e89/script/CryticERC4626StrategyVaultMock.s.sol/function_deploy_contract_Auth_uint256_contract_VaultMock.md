# Function: deploy(contract Auth,uint256,contract VaultMock)

**Contract**: [script/CryticERC4626StrategyVaultMock.s.sol/contract_CryticERC4626StrategyVaultMockScript.md]

## Metadata

- **Contract**: CryticERC4626StrategyVaultMockScript
- **Signature**: `deploy(contract Auth,uint256,contract VaultMock)`
- **Visibility**: public
- **Source Range**: 1437:1276:131

## Implementation

```solidity
function deploy(Auth auth_, uint256 firstDepositAmount_, VaultMock vault_) public returns (CryticERC4626StrategyVaultMock cryticERC4626StrategyVaultMock) {
    string memory name = string.concat("Very Liquid ", vault_.name(), " Strategy Mock Vault");
    string memory symbol = string.concat("vlv", vault_.symbol(), "Mock");
    address implementation = address(new CryticERC4626StrategyVaultMock());
    bytes memory initializationData = abi.encodeCall(ERC4626StrategyVault.initialize, (auth_, name, symbol, fundingAccount, firstDepositAmount_, vault_));
    bytes memory creationCode = abi.encodePacked(type(ERC1967Proxy).creationCode, abi.encode(implementation, initializationData));
    bytes32 salt = keccak256(initializationData);
    cryticERC4626StrategyVaultMock = CryticERC4626StrategyVaultMock(create2Deployer.computeAddress(salt, keccak256(creationCode)));
    IERC20Metadata(address(vault_.asset())).forceApprove(address(cryticERC4626StrategyVaultMock), firstDepositAmount_);
    create2Deployer.deploy(0, salt, abi.encodePacked(type(ERC1967Proxy).creationCode, abi.encode(implementation, initializationData)));
}
```

## External Calls

- **VaultMock::name()**
- **VaultMock::symbol()**
- **ICreate2Deployer::computeAddress(bytes32,bytes32)**
- **IERC20Metadata::forceApprove(contract IERC20,address,uint256)**
- **VaultMock::asset()**
- **ICreate2Deployer::deploy(uint256,bytes32,bytes)**

## State Variable Reads

- **fundingAccount** (`address`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: CryticERC4626StrategyVaultMockScript.deploy(contract Auth,uint256,contract VaultMock) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
```
