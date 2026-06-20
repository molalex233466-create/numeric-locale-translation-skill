# Numeric Locale Translation Skill

**Version:** v0.1
**Status:** experimental

## Purpose

Use this skill when translating, extracting, normalizing, formatting or validating numbers between English and Russian text, Excel/XLSX and CSV.

Numeric locale is part of translation. Do not treat numbers as plain text.

## EN -> machine

English numeric format:

* comma `,` = thousands separator;
* dot `.` = decimal separator.

Convert to machine numeric value:

* `1,800` -> `1800`
* `13,800` -> `13800`
* `250,000` -> `250000`
* `1,800.50` -> `1800.50`

## EN -> RU text

For Russian text output:

* replace English thousands comma with space;
* replace English decimal dot with comma.

Examples:

* `1,800` -> `1 800`
* `13,800` -> `13 800`
* `250,000` -> `250 000`
* `1.5` -> `1,5`
* `23.75` -> `23,75`
* `1,800.50` -> `1 800,50`

## RU -> machine

Russian numeric format:

* space / non-breaking space = thousands separator;
* comma `,` = decimal separator.

Convert to machine numeric value:

* `1 800` -> `1800`
* `13 800` -> `13800`
* `250 000` -> `250000`
* `1,5` -> `1.5`
* `23,75` -> `23.75`
* `1 800,50` -> `1800.50`

## RU -> EN text

For English text output:

* replace Russian thousands space with comma;
* replace Russian decimal comma with dot.

Examples:

* `1 800` -> `1,800`
* `13 800` -> `13,800`
* `250 000` -> `250,000`
* `1,5` -> `1.5`
* `23,75` -> `23.75`
* `1 800,50` -> `1,800.50`

## Excel/XLSX

For Excel/XLSX:

* store values as numbers, not as formatted text;
* use display formatting for locale-specific appearance;
* Russian display: `1800.50` -> `1 800,50`;
* English display: `1800.50` -> `1,800.50`.

## CSV

If decimal comma is used, do not use comma as a column delimiter without explicit escaping. Prefer semicolon `;` for Russian CSV.

## Checks

Preserve the raw value and add `CHECK_NUMERIC_LOCALE` if:

* decimal/thousands separator is ambiguous;
* mixed numeric locales appear in one dataset;
* digit grouping is irregular;
* conversion may change value scale by 10/100/1000;
* a numeric value intended for calculation is stored as text;
* currency, unit, percent, range or comparison sign is lost during parsing.

## Format control lines as code blocks

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
