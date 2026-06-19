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
