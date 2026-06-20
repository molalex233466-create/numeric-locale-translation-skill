# Tests

Test set for `Numeric Locale Translation Skill`.

**Version:** v0.1  
**Status:** experimental

## 1. English -> machine

| ID | Input | Context | Expected machine value | Status |
|---|---:|---|---:|---|
| EN-M-001 | `1,800` | EN number | `1800` | OK |
| EN-M-002 | `6,000` | EN number | `6000` | OK |
| EN-M-003 | `13,800` | EN number | `13800` | OK |
| EN-M-004 | `250,000` | EN number | `250000` | OK |
| EN-M-005 | `1.5` | EN decimal | `1.5` | OK |
| EN-M-006 | `23.75` | EN decimal | `23.75` | OK |
| EN-M-007 | `1,800.50` | EN thousands + decimal | `1800.50` | OK |
| EN-M-008 | `$23,500.75` | EN currency | `23500.75` | OK |

## 2. English -> Russian text

| ID | Input | Context | Expected Russian text | Status |
|---|---:|---|---:|---|
| EN-RU-001 | `1,800` | EN number | `1 800` | OK |
| EN-RU-002 | `6,000` | EN number | `6 000` | OK |
| EN-RU-003 | `13,800` | EN number | `13 800` | OK |
| EN-RU-004 | `250,000` | EN number | `250 000` | OK |
| EN-RU-005 | `1.5` | EN decimal | `1,5` | OK |
| EN-RU-006 | `23.75` | EN decimal | `23,75` | OK |
| EN-RU-007 | `1,800.50` | EN thousands + decimal | `1 800,50` | OK |
| EN-RU-008 | `$23,500.75` | EN currency | `23 500,75 USD` | OK |

## 3. Russian -> machine

| ID | Input | Context | Expected machine value | Status |
|---|---:|---|---:|---|
| RU-M-001 | `1 800` | RU number | `1800` | OK |
| RU-M-002 | `13 800` | RU number | `13800` | OK |
| RU-M-003 | `250 000` | RU number | `250000` | OK |
| RU-M-004 | `1,5` | RU decimal | `1.5` | OK |
| RU-M-005 | `23,75` | RU decimal | `23.75` | OK |
| RU-M-006 | `1 800,50` | RU thousands + decimal | `1800.50` | OK |
| RU-M-007 | `23 500,75 USD` | RU currency | `23500.75` | OK |

## 4. Russian -> English text

| ID | Input | Context | Expected English text | Status |
|---|---:|---|---:|---|
| RU-EN-001 | `1 800` | RU number | `1,800` | OK |
| RU-EN-002 | `13 800` | RU number | `13,800` | OK |
| RU-EN-003 | `250 000` | RU number | `250,000` | OK |
| RU-EN-004 | `1,5` | RU decimal | `1.5` | OK |
| RU-EN-005 | `23,75` | RU decimal | `23.75` | OK |
| RU-EN-006 | `1 800,50` | RU thousands + decimal | `1,800.50` | OK |
| RU-EN-007 | `23 500,75 USD` | RU currency | `USD 23,500.75` | OK |

## 5. Excel/XLSX

| ID | Input | Target display locale | Expected stored value | Expected display | Status |
|---|---:|---|---:|---:|---|
| XLSX-001 | `1,800.50` | Russian | `1800.50` as number | `1 800,50` | OK |
| XLSX-002 | `1 800,50` | English | `1800.50` as number | `1,800.50` | OK |
| XLSX-003 | `23,500.75` | Russian | `23500.75` as number | `23 500,75` | OK |
| XLSX-004 | `23 500,75` | English | `23500.75` as number | `23,500.75` | OK |

Requirement:

`Expected stored value` must be numeric, not text.

## 6. CSV

| ID | Input | Context | Expected action | Status |
|---|---|---|---|---|
| CSV-001 | `price;23 500,75` | RU decimal comma, semicolon delimiter | parse value as `23500.75` | OK |
| CSV-002 | `price,23500.75` | EN decimal dot, comma delimiter | parse value as `23500.75` if CSV fields are valid | OK |
| CSV-003 | `price,23 500,75` | RU decimal comma, comma delimiter | `CHECK_NUMERIC_LOCALE` unless fields are escaped | CHECK |

## 7. Ambiguous values

| ID | Input | Context | Expected action | Reason |
|---|---:|---|---|---|
| AMB-001 | `1,234` | unknown locale | `CHECK_NUMERIC_LOCALE` | may mean `1234` or `1.234` |
| AMB-002 | `1.234` | unknown locale | `CHECK_NUMERIC_LOCALE` | may mean `1234` or `1.234` |
| AMB-003 | `12,34` | unknown locale | `CHECK_NUMERIC_LOCALE` | likely decimal comma, but source locale is not explicit |
| AMB-004 | `12.34` | unknown locale | `CHECK_NUMERIC_LOCALE` | likely decimal dot, but source locale is not explicit |
| AMB-005 | `1,23,456` | unknown locale | `CHECK_NUMERIC_LOCALE` | irregular grouping for EN/RU |
| AMB-006 | `1 23 456` | unknown locale | `CHECK_NUMERIC_LOCALE` | irregular grouping for RU |

## 8. Mixed locale dataset

| ID | Input dataset | Expected action |
|---|---|---|
| MIX-001 | `1,234.56`; `1 234,56`; `1.234,56` | `CHECK_NUMERIC_LOCALE` |
| MIX-002 | `$1,234.56`; `1 234,56 ₽` | preserve raw values and normalize only after target locale is defined |

## 9. Loss checks

| ID | Input | Wrong result | Expected action |
|---|---|---|---|
| LOSS-001 | `$23,500.75` | `23500.75` without currency metadata | preserve currency or add check |
| LOSS-002 | `1,800 packs/hour` | `1800` without unit metadata | preserve unit or add check |
| LOSS-003 | `10%` | `10` without percent metadata | preserve percent sign or add check |
| LOSS-004 | `1,800-2,500` | one value only | preserve range or add check |
| LOSS-005 | `>1,800` | `1800` without comparison sign | preserve comparison sign or add check |

## Control lines

```text
TEST_SET_VERSION=v0.1
NUMERIC_LOCALE_TRANSLATION_RULE=ON
CHECK_NUMERIC_LOCALE_TESTS_INCLUDED=yes
XLSX_NUMERIC_STORAGE_TESTS_INCLUDED=yes
CSV_DELIMITER_TESTS_INCLUDED=yes
AMBIGUOUS_VALUE_TESTS_INCLUDED=yes
LOSS_CHECK_TESTS_INCLUDED=yes
```
