# Numeric Locale Translation Skill

**Version:** v0.1  
**Status:** experimental

## Purpose

Use this skill when translating, extracting, normalizing, formatting, or validating numbers between English and Russian text, Excel/XLSX, and CSV.

Numeric locale is part of translation. Do not treat numbers as plain text.

## When to apply

Apply this skill when a task includes any of the following:

- translation between English and Russian;
- numeric data extraction from text, tables, web pages, PDF, DOCX, HTML, CSV, or XLSX;
- prices, quantities, percentages, dimensions, weights, speeds, rates, capacities, dates with numeric parts, or measurements;
- Excel/XLSX output;
- CSV output;
- ambiguity between decimal and thousands separators.

## Core principle

Always separate three things:

1. raw value — the original text exactly as found;
2. machine value — a normalized numeric value for calculation;
3. display value — a locale-specific human-readable representation.

Do not silently guess when the numeric locale is ambiguous.

## English numeric format

English numeric format normally uses:

- comma `,` as thousands separator;
- dot `.` as decimal separator.

Examples:

- `1,800`
- `13,800`
- `250,000`
- `1.5`
- `23.75`
- `1,800.50`

## Russian numeric format

Russian numeric format normally uses:

- space or non-breaking space as thousands separator;
- comma `,` as decimal separator.

Examples:

- `1 800`
- `13 800`
- `250 000`
- `1,5`
- `23,75`
- `1 800,50`

## EN -> machine

Convert English numeric text to machine numeric value:

| Input | Machine value |
|---:|---:|
| `1,800` | `1800` |
| `6,000` | `6000` |
| `13,800` | `13800` |
| `250,000` | `250000` |
| `1.5` | `1.5` |
| `23.75` | `23.75` |
| `1,800.50` | `1800.50` |
| `$23,500.75` | `23500.75` |

Rules:

- remove English thousands commas;
- keep decimal dot for machine value;
- preserve currency, unit, percent sign, range, and comparison signs as metadata or adjacent fields.

## EN -> RU text

For Russian text output:

- replace English thousands comma with space;
- replace English decimal dot with comma;
- translate or preserve units according to the main translation task.

Examples:

| English input | Russian text output |
|---:|---:|
| `1,800` | `1 800` |
| `6,000` | `6 000` |
| `13,800` | `13 800` |
| `250,000` | `250 000` |
| `1.5` | `1,5` |
| `23.75` | `23,75` |
| `1,800.50` | `1 800,50` |
| `$23,500.75` | `23 500,75 USD` |

## RU -> machine

Convert Russian numeric text to machine numeric value:

| Input | Machine value |
|---:|---:|
| `1 800` | `1800` |
| `13 800` | `13800` |
| `250 000` | `250000` |
| `1,5` | `1.5` |
| `23,75` | `23.75` |
| `1 800,50` | `1800.50` |
| `23 500,75 USD` | `23500.75` |

Rules:

- remove spaces and non-breaking spaces used as thousands separators;
- convert Russian decimal comma to machine decimal dot;
- preserve currency, unit, percent sign, range, and comparison signs as metadata or adjacent fields.

## RU -> EN text

For English text output:

- replace Russian thousands space with comma;
- replace Russian decimal comma with dot;
- translate or preserve units according to the main translation task.

Examples:

| Russian input | English text output |
|---:|---:|
| `1 800` | `1,800` |
| `13 800` | `13,800` |
| `250 000` | `250,000` |
| `1,5` | `1.5` |
| `23,75` | `23.75` |
| `1 800,50` | `1,800.50` |
| `23 500,75 USD` | `USD 23,500.75` |

## Excel/XLSX

For Excel/XLSX output:

- store values as numbers, not as formatted text;
- use display formatting for locale-specific appearance;
- preserve raw source values when locale or scale risk exists;
- keep units, currencies, percent signs, ranges, and comparison signs in separate fields when needed.

Expected storage and display:

| Source | Stored numeric value | Russian display | English display |
|---:|---:|---:|---:|
| `1,800.50` | `1800.50` | `1 800,50` | `1,800.50` |
| `1 800,50` | `1800.50` | `1 800,50` | `1,800.50` |
| `$23,500.75` | `23500.75` | `23 500,75` | `23,500.75` |

Do not store `"1 800,50"` or `"1,800.50"` as text if the value must be used for calculations.

## CSV

CSV requires explicit delimiter handling.

Rules:

- if decimal comma is used, do not use comma as column delimiter without explicit escaping;
- prefer semicolon `;` for Russian CSV with decimal comma;
- if decimal dot is used, comma-delimited CSV is acceptable when fields are valid;
- preserve raw values when delimiter ambiguity exists.

Examples:

| CSV text | Expected handling |
|---|---|
| `price;23 500,75` | parse value as `23500.75` |
| `price,23500.75` | parse value as `23500.75` |
| `price,23 500,75` | add `CHECK_NUMERIC_LOCALE` unless fields are explicitly escaped |

## Ambiguity handling

Add `CHECK_NUMERIC_LOCALE` and preserve the raw value if:

- the source locale is unknown;
- decimal and thousands separators are ambiguous;
- both `,` and `.` appear in a pattern not clearly matching EN or RU;
- groups of digits have irregular length;
- one dataset mixes multiple numeric locale patterns without explicit labeling;
- conversion may change the value scale by 10, 100, or 1000;
- a value intended for calculation is stored as text;
- currency, unit, percent sign, range, or comparison sign is lost during parsing.

Examples of ambiguous values without context:

| Input | Ambiguity |
|---:|---|
| `1,234` | may mean `1234` in EN or `1.234` as decimal comma |
| `1.234` | may mean `1234` in some locales or `1.234` as decimal dot |
| `12,34` | likely decimal comma, but source locale must be known |
| `12.34` | likely decimal dot, but source locale must be known |

## Required output behavior

When using this skill, the assistant should provide:

- translated text with target-locale number formatting;
- machine numeric values for calculations and spreadsheets;
- raw values when needed for auditability;
- `CHECK_NUMERIC_LOCALE` for ambiguous or risky cases.

## Control lines

```text
NUMERIC_LOCALE_TRANSLATION_RULE=ON
EN_THOUSANDS_SEPARATOR_COMMA=yes
EN_DECIMAL_SEPARATOR_DOT=yes
RU_THOUSANDS_SEPARATOR_SPACE=yes
RU_DECIMAL_SEPARATOR_COMMA=yes
XLSX_STORE_NUMBERS_AS_NUMBERS=yes
TEXT_OUTPUT_FORMAT_BY_TARGET_LOCALE=yes
RAW_VALUE_REQUIRED=when_locale_or_scale_risk_exists
CHECK_NUMERIC_LOCALE=when_ambiguous_or_scale_suspicious
```
