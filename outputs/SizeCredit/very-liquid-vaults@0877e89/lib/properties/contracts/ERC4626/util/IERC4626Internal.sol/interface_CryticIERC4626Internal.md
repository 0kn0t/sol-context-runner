# Interface: CryticIERC4626Internal

## Metadata

- **Name**: CryticIERC4626Internal
- **Type**: Interface
- **Path**: lib/properties/contracts/ERC4626/util/IERC4626Internal.sol
- **Documentation**: @notice Developers may optionally implement these interfaces on their Vault contract to increase coverage/enable rounding tests.

## Public/External Functions

### recognizeProfit(uint256)

- **Signature**: `recognizeProfit(uint256)`
- **Visibility**: external
- **Source Range**: 383:50:113

**Signature:**
```solidity
/// @notice Called by the fuzzer. The vault implementation should use TestERC20Token.mint() to credit itself with the amount of profit.
function recognizeProfit(uint256 profit) external;;
```

### recognizeLoss(uint256)

- **Signature**: `recognizeLoss(uint256)`
- **Visibility**: external
- **Source Range**: 582:46:113

**Signature:**
```solidity
/// @notice Called by the fuzzer. The vault implementation should use TestERC20Token.burn()/.transfer() to account for the amount of loss.
function recognizeLoss(uint256 loss) external;;
```
