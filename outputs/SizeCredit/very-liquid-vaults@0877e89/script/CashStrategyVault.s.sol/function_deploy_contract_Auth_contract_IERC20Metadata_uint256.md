# Function: deploy(contract Auth,contract IERC20Metadata,uint256)

**Contract**: [script/CashStrategyVault.s.sol/contract_CashStrategyVaultScript.md]

## Metadata

- **Contract**: CashStrategyVaultScript
- **Signature**: `deploy(contract Auth,contract IERC20Metadata,uint256)`
- **Visibility**: public
- **Source Range**: 1257:1115:127

## Implementation

```solidity
function deploy(Auth auth_, IERC20Metadata asset_, uint256 firstDepositAmount_) public returns (CashStrategyVault cashStrategyVault) {
    string memory name = string.concat("Very Liquid Cash ", asset_.name(), " Strategy Vault");
    string memory symbol = string.concat("vlv", "Cash", asset_.symbol());
    address implementation = address(new CashStrategyVault());
    bytes memory initializationData = abi.encodeCall(BaseVault.initialize, (auth_, asset_, name, symbol, fundingAccount, firstDepositAmount_));
    bytes memory creationCode = abi.encodePacked(type(ERC1967Proxy).creationCode, abi.encode(implementation, initializationData));
    bytes32 salt = keccak256(initializationData);
    cashStrategyVault = CashStrategyVault(create2Deployer.computeAddress(salt, keccak256(creationCode)));
    asset_.forceApprove(address(cashStrategyVault), firstDepositAmount_);
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
┌─ [0] ⚙️ FUNCTION: CashStrategyVaultScript.deploy(contract Auth,contract IERC20Metadata,uint256) (NodeID: 0)
    💬 Args: [no args]
    👁️  Def: public
```
