# Output Schema — Recipe → Allergen Matrix

This file is the contract. Every run produces every section below, in this order, with these exact headings, regardless of what the input looks like. A section with nothing to report still appears and says so.

---

## Allergen Set

The union of the US FDA major food allergens (9) and the EU/UK declarable allergens (14). The EU/UK category "cereals containing gluten" is split into Wheat and Other gluten cereals so the US-regulated allergen stays visible on its own row. Always in this order:

| # | Allergen | US FDA | EU/UK | Scope note |
|---|---|---|---|---|
| 1 | Milk | ✓ | ✓ | |
| 2 | Egg | ✓ | ✓ | |
| 3 | Fish | ✓ | ✓ | |
| 4 | Crustacean shellfish | ✓ | ✓ | Shrimp, crab, lobster, crayfish. |
| 5 | Molluscs | | ✓ | Clams, mussels, oysters, scallops, squid, octopus, snails. |
| 6 | Tree nuts | ✓ | ✓ | Which items count is defined row by row in `lookup-table.md`. |
| 7 | Peanuts | ✓ | ✓ | |
| 8 | Wheat | ✓ | ✓ | Includes spelt, kamut, durum, semolina. |
| 9 | Other gluten cereals | | ✓ | Barley, rye, oats. |
| 10 | Soybeans | ✓ | ✓ | |
| 11 | Sesame | ✓ | ✓ | |
| 12 | Mustard | | ✓ | |
| 13 | Celery | | ✓ | Includes celeriac. |
| 14 | Lupin | | ✓ | |
| 15 | Sulphites | | ✓ | EU/UK declaration depends on concentration. Recipes do not state concentration, so this row reflects ingredient type only. |

Allergens outside this set (e.g., corn, coconut) are never placed in the matrix. When a table row names one, it is recorded as an out-of-scope note in Line Accounting.

---

## Cell Values

Each matrix cell holds exactly one of these three values. No other wording is permitted.

| Value | Meaning |
|---|---|
| **Present** | At least one ingredient term matches a table row that assigns this allergen. |
| **Undetermined** | No term assigns this allergen, but at least one term matches a table row that flags it as possible (brand-dependent, variable, or a category term). |
| **Not found in listed ingredients** | No term assigns or flags this allergen. This is not a claim that the dish is free of it. |

**Precedence:** Present > Undetermined > Not found in listed ingredients.

---

## Line Statuses

Every ingredient term receives exactly one status. A line with no ingredient term is `NO-INGREDIENT`. Term IDs are defined in `rules.md`.

| Status | Meaning |
|---|---|
| `MATCHED-ALLERGEN` | Matches a table row that assigns one or more allergens (it may also flag others as possible). |
| `MATCHED-POSSIBLE` | Matches a table row that assigns no allergen but flags one or more as possible. |
| `MATCHED-NONE` | Matches a table row that assigns no allergen in the set. |
| `UNRESOLVED` | An ingredient term that matches no table row. |
| `NO-INGREDIENT` | A line with no new ingredient term (e.g., a title, a heading, a serving note, an instruction that names no ingredient or only refers to ingredients on earlier lines). |

---

## Output Sections

Sections appear in this order. The first half answers the question; the second half is the audit trail that proves it.

### Section 0 — Header

```
RECIPE: <recipe name exactly as written> (<L-number or "user message">), or "Not in source"
        — when more than one title candidate exists, the line above is followed by:
          — unconfirmed; candidates: <other candidate L-numbers, up to three>
SOURCE: <source exactly as written> (<L-number or "user message">), or "Not in source"
TABLE: lookup-table <version from lookup-table.md>
STATUS: <status line — see below>
```

The STATUS line is exactly one of:

- `STATUS: RESOLVED — all ingredient terms matched.`
- `STATUS: PROVISIONAL — <N> unresolved term(s): <term IDs>. "Not found" cells are provisional until reviewed.`
- `STATUS: NO INGREDIENTS FOUND — the input contains no ingredient terms.`

### Section 1 — At a Glance

Built only from the Allergen Matrix and the Review Queue. This section decides nothing on its own.

```
Present: <allergen (terms)> · <allergen (terms)> ...
Undetermined: <allergen (terms)> · <allergen (terms)> ...
Needs review: <N> item(s)
- <term> (<term IDs>) — <action>
- <term> (<term IDs>) — <action>
```

A term marked `Alternative`, `Optional`, or `Serving` in Line Accounting carries an asterisk in the Present and Undetermined lines. When any asterisk appears, this fixed footnote line follows those two lines, exactly as written:

```
Note (*): Listed as optional, as an alternative, or as something the dish is served with. May not be used in every preparation — confirm which ingredients were used.
```

- **Present** lists every allergen whose matrix cell is Present; **Undetermined** lists every allergen whose cell is Undetermined. Both follow matrix order.
- After each Present allergen, the parentheses hold the distinct terms, exactly as written in the input, whose matched rows assign it. After each Undetermined allergen, they hold the terms that flag it as possible. A term that only flags an allergen never appears after a Present allergen. Example: `Other gluten cereals (rolled oats)`.
- A line with no allergens reads `—`. It never reads "none" or "free."
- **Needs review** gives the number of Review Queue items, followed by one line per distinct term and action. Queue items with the same term (ignoring capitalization) and the same action share one line, with all their term IDs: `- yellow mustard (L10, L18) — Check supplier label`. If the queue is empty, the line reads `Needs review: —` with nothing below it.

### Section 2 — Allergen Matrix

Fifteen rows, fixed order, three columns.

```
| Allergen | Cell | Source |
|---|---|---|
| Milk | <cell value> | <evidence> |
| Egg | <cell value> | <evidence> |
| Fish | <cell value> | <evidence> |
| Crustacean shellfish | <cell value> | <evidence> |
| Molluscs | <cell value> | <evidence> |
| Tree nuts | <cell value> | <evidence> |
| Peanuts | <cell value> | <evidence> |
| Wheat | <cell value> | <evidence> |
| Other gluten cereals | <cell value> | <evidence> |
| Soybeans | <cell value> | <evidence> |
| Sesame | <cell value> | <evidence> |
| Mustard | <cell value> | <evidence> |
| Celery | <cell value> | <evidence> |
| Lupin | <cell value> | <evidence> |
| Sulphites | <cell value> | <evidence> |
```

Evidence format:

- **Present or Undetermined:** `<term ID> → T-<row ID>` for each supporting term, separated by `; `. Terms flagging the allergen as possible are tagged `(possible)`. Example (Fish row): `L1 → T-012; L5 → T-017 (possible)`
- **Not found in listed ingredients, STATUS RESOLVED:** `—`
- **Not found in listed ingredients, STATUS PROVISIONAL:** `— · open: <term IDs of unresolved terms>`. Example: `— · open: L9`
- **Not found in listed ingredients, STATUS NO INGREDIENTS FOUND:** `—`

### Section 3 — Review Queue

Every term with status `UNRESOLVED`, and every term whose matched table row lists any allergen under Possible (including `MATCHED-ALLERGEN` terms such as soy sauce), numbered in term order.

```
1. <term ID> — "<term>" — <action>
```

Action is exactly one of:

- `Check supplier label`
- `Confirm brand`
- `Check house recipe`
- `Clarify ingredient`

If there are none: `None.`

### Section 4 — Line Accounting

Every L-number from Section 5 appears exactly once in Line Accounting, either in its own row or in the grouped row.

- **Own row, in line order:** every ingredient term, and every line that contains references but no new terms.
- **Grouped row, always last:** every line with no ingredient term and no reference (titles, headings, step numbers, and any other text), listed as ranges. If there are no such lines, the grouped row's ID is `None`.

```
| ID | Term | Status | Table row | Allergens | Note |
|---|---|---|---|---|---|
| L2 | <term> | <status> | T-<ID> or — | <allergens or —> | <note or —> |
| L3.1 | <term> | <status> | T-<ID> or — | <allergens or —> | <note or —> |
| L3.2 | <term> | <status> | T-<ID> or — | <allergens or —> | <note or —> |
| L6 | — | NO-INGREDIENT | — | — | References <L-numbers> |
| L1, L4–L5, L7 | — | NO-INGREDIENT | — | — | No ingredient mentions |
```

The Term column holds the ingredient term exactly as it appears in the input. The Note column holds only: `Alternative`, `Optional`, or `Serving` for a conditional term, an out-of-scope allergen named by the table row, the table row's stated reason for a possible flag, `References <L-numbers>`, `No ingredient mentions` (grouped row only), or `—`. Multiple notes are separated by `; `.

### Section 5 — Numbered Source

Every line of the input, verbatim, numbered `L1` through `Ln`, including instruction lines. Spelling, capitalization, and quantities are reproduced exactly as they appear in the input.

```
L1  <verbatim text>
L2  <verbatim text>
...
```

### Section 6 — Scope Statement

Fixed text, reproduced exactly:

> This matrix reports allergens identified in the listed ingredients using the stated lookup table version. It covers the US FDA major food allergens and the EU/UK declarable allergens only. It does not assess cross-contact, supplier formulation changes, sulphite concentration, or the preparation environment. It is not an allergen certification. Undetermined cells and unresolved terms require human review before this information is shared with guests.

---

## Blank Shape

The skeleton every output fills. A reader can hold any output up against this and check that nothing is missing or reordered.

```
RECIPE:
SOURCE:
TABLE:
STATUS:

## At a Glance
Present:
Undetermined:
<footnote line, only if an asterisk appears above>
Needs review:
- 

## Allergen Matrix
| Allergen | Cell | Source |
|---|---|---|
| Milk | | |
| Egg | | |
| Fish | | |
| Crustacean shellfish | | |
| Molluscs | | |
| Tree nuts | | |
| Peanuts | | |
| Wheat | | |
| Other gluten cereals | | |
| Soybeans | | |
| Sesame | | |
| Mustard | | |
| Celery | | |
| Lupin | | |
| Sulphites | | |

## Review Queue

## Line Accounting
| ID | Term | Status | Table row | Allergens | Note |
|---|---|---|---|---|---|
| <grouped lines or None> | — | NO-INGREDIENT | — | — | No ingredient mentions |

## Numbered Source
L1

## Scope Statement
```
