# Examples

These examples assume that `SKILL.md` is attached to the chat, added to the project knowledge, or otherwise provided as an active instruction.

## Example 1: English text -> Russian text

User prompt:

```text
Apply the attached Numeric Locale Translation Skill.

Translate into Russian:
The machine speed is 1,800 packs/hour and the price is $23,500.75.
```

Expected answer:

```text
Скорость машины составляет 1 800 упаковок/час, цена — 23 500,75 USD.
```

Expected numeric handling:

```text
1,800 -> 1 800
23,500.75 -> 23 500,75
```

## Example 2: English text -> Russian Excel/XLSX

User prompt:

```text
Apply the attached Numeric Locale Translation Skill.

Convert the following English data into a Russian Excel/XLSX table:

Item: Packaging machine
Speed: 1,800 packs/hour
Price: $23,500.75

Create an XLSX file.
```

Expected result:

The assistant creates an XLSX file.

Expected stored values:

| Field | Stored value type | Stored value |
| ----- | ----------------: | -----------: |
| Speed |            number |         1800 |
| Price |            number |     23500.75 |

Expected Russian display:

| Field |   Display |
| ----- | --------: |
| Speed |     1 800 |
| Price | 23 500,75 |

Expected rule:

```text
Do not store "1 800" or "23 500,75" as text if the values must be used for calculations.
```

## Example 3: Russian text -> English text

User prompt:

```text
Apply the attached Numeric Locale Translation Skill.

Translate into English:
Скорость машины составляет 1 800 упаковок/час, цена — 23 500,75 USD.
```

Expected answer:

```text
The machine speed is 1,800 packs/hour, and the price is USD 23,500.75.
```

Expected numeric handling:

```text
1 800 -> 1,800
23 500,75 -> 23,500.75
```

## Example 4: Russian text -> English Excel/XLSX

User prompt:

```text
Apply the attached Numeric Locale Translation Skill.

Convert the following Russian data into an English Excel/XLSX table:

Позиция: Упаковочная машина
Скорость: 1 800 упаковок/час
Цена: 23 500,75 USD

Create an XLSX file.
```

Expected result:

The assistant creates an XLSX file.

Expected stored values:

| Field | Stored value type | Stored value |
| ----- | ----------------: | -----------: |
| Speed |            number |         1800 |
| Price |            number |     23500.75 |

Expected English display:

| Field |   Display |
| ----- | --------: |
| Speed |     1,800 |
| Price | 23,500.75 |

## Example 5: Ambiguous numeric locale

User prompt:

```text
Apply the attached Numeric Locale Translation Skill.

Normalize this value:
1,234
```

Expected action:

```text
CHECK_NUMERIC_LOCALE
```

Expected explanation:

```text
The value is ambiguous without source locale or context:
- English format may mean 1234.
- Russian/European decimal format may mean 1.234.
Preserve the raw value and ask for source locale or context.
```

## Example 6: Mixed numeric locales

User prompt:

```text
Apply the attached Numeric Locale Translation Skill.

Normalize this dataset:
A: 1,234.56
B: 1 234,56
C: 1.234,56
```

Expected action:

```text
CHECK_NUMERIC_LOCALE
```

Expected explanation:

```text
The dataset contains mixed numeric locale patterns. Preserve raw values and normalize only after source locale or target locale is defined.
```

