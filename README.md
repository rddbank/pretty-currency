# pretty-currency

Library for pretty print crypto currency values with K, M, B, T suffixes.

## Installation

```bash
npm install @immersedin/pretty-currency
```

## Usage

```javascript
import { cryptoPretty } from '@immersedin/pretty-currency';

// Basic usage
const result = cryptoPretty(1234567);
console.log(result.shortVariant()); // "1.23M"
console.log(result.value); // "1234567"

// Small values
const small = cryptoPretty(0.0001234);
console.log(small.shortVariant()); // Formatted small number
console.log(small.value); // "0.0001234"

// Negative values
const negative = cryptoPretty(-5000);
console.log(negative.shortVariant()); // "-5K"

// Custom decimals
const custom = cryptoPretty(1234567, undefined, 3);
console.log(custom.shortVariant()); // Custom decimal precision

// Custom suffixes
const customSuffix = cryptoPretty(5000, { 0: "", 3: "k", 6: "m" });
console.log(customSuffix.shortVariant()); // "5k"
```

## Features

- Formats large numbers with K, M, B, T suffixes
- Handles small numbers (< 1) with scientific notation
- Supports negative numbers
- Customizable decimal precision
- Customizable suffix mapping
- Maximum supported value: 99.99T (99,999,999,999,999)

## API

### `cryptoPretty(num, classValues, decimals)`

**Parameters:**

- `num` (number): The number to format (required)
- `classValues` (object): Custom suffix mapping (optional)
  - Default: `{ 0: "", 4: "K", 7: "M", 10: "B", 13: "T" }`
- `decimals` (number): Number of decimal places (optional)
  - Default: 2

**Returns:**

An object with:
- `shortVariant()`: Function that returns the formatted string
- `value`: The original number as a string

## Examples

```javascript
// Large numbers
cryptoPretty(1000).shortVariant();      // "1K"
cryptoPretty(1000000).shortVariant();   // "1M"
cryptoPretty(1000000000).shortVariant(); // "1B"
cryptoPretty(1000000000000).shortVariant(); // "1T"

// Edge cases
cryptoPretty(0).shortVariant();         // "0"
cryptoPretty(999).shortVariant();       // "999"
cryptoPretty(99999999999999).shortVariant(); // "99.99T"
```

## Limitations

- Maximum value: 99,999,999,999,999 (99.99T)
- Minimum value: Supports very small numbers (1e-15 and smaller)
- The `value` property may lose the negative sign for negative numbers (known issue)

## License

MIT

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.
