# Contract: StringMap

## Metadata

- **Name**: StringMap
- **Type**: Contract
- **Path**: lib/safe-utils/lib/solidity-http/src/StringMap.sol

## Structs

### StringToStringMap

```solidity
struct StringToStringMap {
    StringSet.Set _keys;
    mapping(string => string) _values;
}
```

## Errors

### EnumerableMapNonexistentKey

```solidity
///  @dev Query for a nonexistent map key.
error EnumerableMapNonexistentKey(string key);
```
