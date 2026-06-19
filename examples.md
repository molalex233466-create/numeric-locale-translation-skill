# Examples

## Example 1: EN text to RU text

Input:

`The machine speed is 1,800 packs/hour and the price is $23,500.75.`

Expected RU text:

`The machine speed is 1 800 packs/hour and the price is $23 500,75.`

## Example 2: EN to machine value

Input:

`1,800.50`

Expected machine value:

`1800.50`

## Example 3: RU text to EN text

Input:

`1 800,50`

Expected EN text:

`1,800.50`

## Example 4: Excel/XLSX

Input:

`1,800.50`

Expected stored value:

`1800.50` as numeric value.

Expected RU display:

`1 800,50`

Expected EN display:

`1,800.50`

## Example 5: Ambiguous value

Input:

`1,234`

Expected action:

Preserve raw value and add `CHECK_NUMERIC_LOCALE` if the source locale is unknown.
