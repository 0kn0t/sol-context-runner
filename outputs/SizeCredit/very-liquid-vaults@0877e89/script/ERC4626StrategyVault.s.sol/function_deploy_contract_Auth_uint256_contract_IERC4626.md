# Function: deploy(contract Auth,uint256,contract IERC4626)

**Contract**: [script/ERC4626StrategyVault.s.sol/contract_ERC4626StrategyVaultScript.md]

## Metadata

- **Contract**: ERC4626StrategyVaultScript
- **Signature**: `deploy(contract Auth,uint256,contract IERC4626)`
- **Visibility**: public
- **Source Range**: 1278:1168:133

## Implementation

```solidity
function deploy(Auth auth_, uint256 firstDepositAmount_, IERC4626 vault_) public returns (ERC4626StrategyVault erc4626StrategyVault) {
    string memory name = string.concat("Very Liquid ", vault_.name(), " Strategy Vault");
    string memory symbol = string.concat("vlv", vault_.symbol());
    address implementation = address(new ERC4626StrategyVault());
    bytes memory initializationData = abi.encodeCall(ERC4626StrategyVault.initialize, (auth_, name, symbol, fundingAccount, firstDepositAmount_, vault_));
    bytes memory creationCode = abi.encodePacked(type(ERC1967Proxy).creationCode, abi.encode(implementation, initializationData));
    bytes32 salt = keccak256(initializationData);
    erc4626StrategyVault = ERC4626StrategyVault(create2Deployer.computeAddress(salt, keccak256(creationCode)));
    IERC20Metadata(address(vault_.asset())).forceApprove(address(erc4626StrategyVault), firstDepositAmount_);
    create2Deployer.deploy(0, salt, abi.encodePacked(type(ERC1967Proxy).creationCode, abi.encode(implementation, initializationData)));
}
```

## External Calls

- **IERC4626::name()**
- **IERC4626::symbol()**
- **ICreate2Deployer::computeAddress(bytes32,bytes32)**
- **IERC20Metadata::forceApprove(contract IERC20,address,uint256)**
- **IERC4626::asset()**
- **ICreate2Deployer::deploy(uint256,bytes32,bytes)**

## State Variable Reads

- **fundingAccount** (`address`)

## Call Tree

```
┌─ [0] ⚙️ FUNCTION: ERC4626StrategyVaultScript.deploy(contract Auth,uint256,contract IERC4626) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
```
