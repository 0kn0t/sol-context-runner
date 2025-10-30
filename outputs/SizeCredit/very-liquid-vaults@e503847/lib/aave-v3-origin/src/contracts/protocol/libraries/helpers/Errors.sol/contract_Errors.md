# Contract: Errors

## Metadata

- **Name**: Errors
- **Type**: Contract
- **Path**: lib/aave-v3-origin/src/contracts/protocol/libraries/helpers/Errors.sol
- **Documentation**:  @title Errors library
   @author Aave
   @notice Defines the error messages emitted by the different contracts of the Aave protocol

## State Variables

### CALLER_NOT_POOL_ADMIN

```solidity
string public constant CALLER_NOT_POOL_ADMIN = "1"
```

### CALLER_NOT_EMERGENCY_ADMIN

```solidity
string public constant CALLER_NOT_EMERGENCY_ADMIN = "2"
```

### CALLER_NOT_POOL_OR_EMERGENCY_ADMIN

```solidity
string public constant CALLER_NOT_POOL_OR_EMERGENCY_ADMIN = "3"
```

### CALLER_NOT_RISK_OR_POOL_ADMIN

```solidity
string public constant CALLER_NOT_RISK_OR_POOL_ADMIN = "4"
```

### CALLER_NOT_ASSET_LISTING_OR_POOL_ADMIN

```solidity
string public constant CALLER_NOT_ASSET_LISTING_OR_POOL_ADMIN = "5"
```

### CALLER_NOT_BRIDGE

```solidity
string public constant CALLER_NOT_BRIDGE = "6"
```

### ADDRESSES_PROVIDER_NOT_REGISTERED

```solidity
string public constant ADDRESSES_PROVIDER_NOT_REGISTERED = "7"
```

### INVALID_ADDRESSES_PROVIDER_ID

```solidity
string public constant INVALID_ADDRESSES_PROVIDER_ID = "8"
```

### NOT_CONTRACT

```solidity
string public constant NOT_CONTRACT = "9"
```

### CALLER_NOT_POOL_CONFIGURATOR

```solidity
string public constant CALLER_NOT_POOL_CONFIGURATOR = "10"
```

### CALLER_NOT_ATOKEN

```solidity
string public constant CALLER_NOT_ATOKEN = "11"
```

### INVALID_ADDRESSES_PROVIDER

```solidity
string public constant INVALID_ADDRESSES_PROVIDER = "12"
```

### INVALID_FLASHLOAN_EXECUTOR_RETURN

```solidity
string public constant INVALID_FLASHLOAN_EXECUTOR_RETURN = "13"
```

### RESERVE_ALREADY_ADDED

```solidity
string public constant RESERVE_ALREADY_ADDED = "14"
```

### NO_MORE_RESERVES_ALLOWED

```solidity
string public constant NO_MORE_RESERVES_ALLOWED = "15"
```

### EMODE_CATEGORY_RESERVED

```solidity
string public constant EMODE_CATEGORY_RESERVED = "16"
```

### INVALID_EMODE_CATEGORY_ASSIGNMENT

```solidity
string public constant INVALID_EMODE_CATEGORY_ASSIGNMENT = "17"
```

### RESERVE_LIQUIDITY_NOT_ZERO

```solidity
string public constant RESERVE_LIQUIDITY_NOT_ZERO = "18"
```

### FLASHLOAN_PREMIUM_INVALID

```solidity
string public constant FLASHLOAN_PREMIUM_INVALID = "19"
```

### INVALID_RESERVE_PARAMS

```solidity
string public constant INVALID_RESERVE_PARAMS = "20"
```

### INVALID_EMODE_CATEGORY_PARAMS

```solidity
string public constant INVALID_EMODE_CATEGORY_PARAMS = "21"
```

### BRIDGE_PROTOCOL_FEE_INVALID

```solidity
string public constant BRIDGE_PROTOCOL_FEE_INVALID = "22"
```

### CALLER_MUST_BE_POOL

```solidity
string public constant CALLER_MUST_BE_POOL = "23"
```

### INVALID_MINT_AMOUNT

```solidity
string public constant INVALID_MINT_AMOUNT = "24"
```

### INVALID_BURN_AMOUNT

```solidity
string public constant INVALID_BURN_AMOUNT = "25"
```

### INVALID_AMOUNT

```solidity
string public constant INVALID_AMOUNT = "26"
```

### RESERVE_INACTIVE

```solidity
string public constant RESERVE_INACTIVE = "27"
```

### RESERVE_FROZEN

```solidity
string public constant RESERVE_FROZEN = "28"
```

### RESERVE_PAUSED

```solidity
string public constant RESERVE_PAUSED = "29"
```

### BORROWING_NOT_ENABLED

```solidity
string public constant BORROWING_NOT_ENABLED = "30"
```

### NOT_ENOUGH_AVAILABLE_USER_BALANCE

```solidity
string public constant NOT_ENOUGH_AVAILABLE_USER_BALANCE = "32"
```

### INVALID_INTEREST_RATE_MODE_SELECTED

```solidity
string public constant INVALID_INTEREST_RATE_MODE_SELECTED = "33"
```

### COLLATERAL_BALANCE_IS_ZERO

```solidity
string public constant COLLATERAL_BALANCE_IS_ZERO = "34"
```

### HEALTH_FACTOR_LOWER_THAN_LIQUIDATION_THRESHOLD

```solidity
string public constant HEALTH_FACTOR_LOWER_THAN_LIQUIDATION_THRESHOLD = "35"
```

### COLLATERAL_CANNOT_COVER_NEW_BORROW

```solidity
string public constant COLLATERAL_CANNOT_COVER_NEW_BORROW = "36"
```

### COLLATERAL_SAME_AS_BORROWING_CURRENCY

```solidity
string public constant COLLATERAL_SAME_AS_BORROWING_CURRENCY = "37"
```

### NO_DEBT_OF_SELECTED_TYPE

```solidity
string public constant NO_DEBT_OF_SELECTED_TYPE = "39"
```

### NO_EXPLICIT_AMOUNT_TO_REPAY_ON_BEHALF

```solidity
string public constant NO_EXPLICIT_AMOUNT_TO_REPAY_ON_BEHALF = "40"
```

### NO_OUTSTANDING_VARIABLE_DEBT

```solidity
string public constant NO_OUTSTANDING_VARIABLE_DEBT = "42"
```

### UNDERLYING_BALANCE_ZERO

```solidity
string public constant UNDERLYING_BALANCE_ZERO = "43"
```

### INTEREST_RATE_REBALANCE_CONDITIONS_NOT_MET

```solidity
string public constant INTEREST_RATE_REBALANCE_CONDITIONS_NOT_MET = "44"
```

### HEALTH_FACTOR_NOT_BELOW_THRESHOLD

```solidity
string public constant HEALTH_FACTOR_NOT_BELOW_THRESHOLD = "45"
```

### COLLATERAL_CANNOT_BE_LIQUIDATED

```solidity
string public constant COLLATERAL_CANNOT_BE_LIQUIDATED = "46"
```

### SPECIFIED_CURRENCY_NOT_BORROWED_BY_USER

```solidity
string public constant SPECIFIED_CURRENCY_NOT_BORROWED_BY_USER = "47"
```

### INCONSISTENT_FLASHLOAN_PARAMS

```solidity
string public constant INCONSISTENT_FLASHLOAN_PARAMS = "49"
```

### BORROW_CAP_EXCEEDED

```solidity
string public constant BORROW_CAP_EXCEEDED = "50"
```

### SUPPLY_CAP_EXCEEDED

```solidity
string public constant SUPPLY_CAP_EXCEEDED = "51"
```

### UNBACKED_MINT_CAP_EXCEEDED

```solidity
string public constant UNBACKED_MINT_CAP_EXCEEDED = "52"
```

### DEBT_CEILING_EXCEEDED

```solidity
string public constant DEBT_CEILING_EXCEEDED = "53"
```

### UNDERLYING_CLAIMABLE_RIGHTS_NOT_ZERO

```solidity
string public constant UNDERLYING_CLAIMABLE_RIGHTS_NOT_ZERO = "54"
```

### VARIABLE_DEBT_SUPPLY_NOT_ZERO

```solidity
string public constant VARIABLE_DEBT_SUPPLY_NOT_ZERO = "56"
```

### LTV_VALIDATION_FAILED

```solidity
string public constant LTV_VALIDATION_FAILED = "57"
```

### INCONSISTENT_EMODE_CATEGORY

```solidity
string public constant INCONSISTENT_EMODE_CATEGORY = "58"
```

### PRICE_ORACLE_SENTINEL_CHECK_FAILED

```solidity
string public constant PRICE_ORACLE_SENTINEL_CHECK_FAILED = "59"
```

### ASSET_NOT_BORROWABLE_IN_ISOLATION

```solidity
string public constant ASSET_NOT_BORROWABLE_IN_ISOLATION = "60"
```

### RESERVE_ALREADY_INITIALIZED

```solidity
string public constant RESERVE_ALREADY_INITIALIZED = "61"
```

### USER_IN_ISOLATION_MODE_OR_LTV_ZERO

```solidity
string public constant USER_IN_ISOLATION_MODE_OR_LTV_ZERO = "62"
```

### INVALID_LTV

```solidity
string public constant INVALID_LTV = "63"
```

### INVALID_LIQ_THRESHOLD

```solidity
string public constant INVALID_LIQ_THRESHOLD = "64"
```

### INVALID_LIQ_BONUS

```solidity
string public constant INVALID_LIQ_BONUS = "65"
```

### INVALID_DECIMALS

```solidity
string public constant INVALID_DECIMALS = "66"
```

### INVALID_RESERVE_FACTOR

```solidity
string public constant INVALID_RESERVE_FACTOR = "67"
```

### INVALID_BORROW_CAP

```solidity
string public constant INVALID_BORROW_CAP = "68"
```

### INVALID_SUPPLY_CAP

```solidity
string public constant INVALID_SUPPLY_CAP = "69"
```

### INVALID_LIQUIDATION_PROTOCOL_FEE

```solidity
string public constant INVALID_LIQUIDATION_PROTOCOL_FEE = "70"
```

### INVALID_EMODE_CATEGORY

```solidity
string public constant INVALID_EMODE_CATEGORY = "71"
```

### INVALID_UNBACKED_MINT_CAP

```solidity
string public constant INVALID_UNBACKED_MINT_CAP = "72"
```

### INVALID_DEBT_CEILING

```solidity
string public constant INVALID_DEBT_CEILING = "73"
```

### INVALID_RESERVE_INDEX

```solidity
string public constant INVALID_RESERVE_INDEX = "74"
```

### ACL_ADMIN_CANNOT_BE_ZERO

```solidity
string public constant ACL_ADMIN_CANNOT_BE_ZERO = "75"
```

### INCONSISTENT_PARAMS_LENGTH

```solidity
string public constant INCONSISTENT_PARAMS_LENGTH = "76"
```

### ZERO_ADDRESS_NOT_VALID

```solidity
string public constant ZERO_ADDRESS_NOT_VALID = "77"
```

### INVALID_EXPIRATION

```solidity
string public constant INVALID_EXPIRATION = "78"
```

### INVALID_SIGNATURE

```solidity
string public constant INVALID_SIGNATURE = "79"
```

### OPERATION_NOT_SUPPORTED

```solidity
string public constant OPERATION_NOT_SUPPORTED = "80"
```

### DEBT_CEILING_NOT_ZERO

```solidity
string public constant DEBT_CEILING_NOT_ZERO = "81"
```

### ASSET_NOT_LISTED

```solidity
string public constant ASSET_NOT_LISTED = "82"
```

### INVALID_OPTIMAL_USAGE_RATIO

```solidity
string public constant INVALID_OPTIMAL_USAGE_RATIO = "83"
```

### UNDERLYING_CANNOT_BE_RESCUED

```solidity
string public constant UNDERLYING_CANNOT_BE_RESCUED = "85"
```

### ADDRESSES_PROVIDER_ALREADY_ADDED

```solidity
string public constant ADDRESSES_PROVIDER_ALREADY_ADDED = "86"
```

### POOL_ADDRESSES_DO_NOT_MATCH

```solidity
string public constant POOL_ADDRESSES_DO_NOT_MATCH = "87"
```

### SILOED_BORROWING_VIOLATION

```solidity
string public constant SILOED_BORROWING_VIOLATION = "89"
```

### RESERVE_DEBT_NOT_ZERO

```solidity
string public constant RESERVE_DEBT_NOT_ZERO = "90"
```

### FLASHLOAN_DISABLED

```solidity
string public constant FLASHLOAN_DISABLED = "91"
```

### INVALID_MAX_RATE

```solidity
string public constant INVALID_MAX_RATE = "92"
```

### WITHDRAW_TO_ATOKEN

```solidity
string public constant WITHDRAW_TO_ATOKEN = "93"
```

### SUPPLY_TO_ATOKEN

```solidity
string public constant SUPPLY_TO_ATOKEN = "94"
```

### SLOPE_2_MUST_BE_GTE_SLOPE_1

```solidity
string public constant SLOPE_2_MUST_BE_GTE_SLOPE_1 = "95"
```

### CALLER_NOT_RISK_OR_POOL_OR_EMERGENCY_ADMIN

```solidity
string public constant CALLER_NOT_RISK_OR_POOL_OR_EMERGENCY_ADMIN = "96"
```

### LIQUIDATION_GRACE_SENTINEL_CHECK_FAILED

```solidity
string public constant LIQUIDATION_GRACE_SENTINEL_CHECK_FAILED = "97"
```

### INVALID_GRACE_PERIOD

```solidity
string public constant INVALID_GRACE_PERIOD = "98"
```

### INVALID_FREEZE_STATE

```solidity
string public constant INVALID_FREEZE_STATE = "99"
```

### NOT_BORROWABLE_IN_EMODE

```solidity
string public constant NOT_BORROWABLE_IN_EMODE = "100"
```

### CALLER_NOT_UMBRELLA

```solidity
string public constant CALLER_NOT_UMBRELLA = "101"
```

### RESERVE_NOT_IN_DEFICIT

```solidity
string public constant RESERVE_NOT_IN_DEFICIT = "102"
```

### MUST_NOT_LEAVE_DUST

```solidity
string public constant MUST_NOT_LEAVE_DUST = "103"
```

### USER_CANNOT_HAVE_DEBT

```solidity
string public constant USER_CANNOT_HAVE_DEBT = "104"
```
