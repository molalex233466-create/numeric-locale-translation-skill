# numeric-locale-translation-skill

**Version:** v0.1
**Status:** experimental

LLM skill for locale-aware numeric translation, parsing, formatting and validation between English and Russian text, Excel/XLSX and CSV.

## Purpose

This skill defines rules for handling numeric locale differences between English and Russian contexts.

It covers:

* English to Russian numeric formatting;
* Russian to English numeric formatting;
* machine-safe numeric normalization;
* Excel/XLSX numeric values and display formats;
* CSV delimiter risks;
* ambiguity checks for decimal and thousands separators.

## Core principle

Do not treat numbers as plain text during translation.

Numeric locale is part of translation. The assistant must preserve numeric value, convert display format according to the target locale, and flag ambiguous cases instead of silently guessing.

## Related work

This skill is not a general localization framework.

General localization and l10n tools usually focus on formatting already-known values according to a target locale: dates, numbers, currencies, UI strings, and regional display conventions.

This skill focuses specifically on numeric locale translation, parsing, formatting, and validation between English and Russian text, Excel/XLSX, and CSV.

Its main purpose is to prevent numeric value errors caused by decimal and thousands separator confusion, such as:

1,800 being incorrectly interpreted as 1.8 or 1;
1 800,50 being stored as text instead of a numeric value;
mixed-locale datasets being normalized silently without a check.

The skill is intended for LLM workflows where numbers are translated, extracted, normalized, or written into tables and spreadsheets.
