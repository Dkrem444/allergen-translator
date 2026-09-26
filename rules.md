# Rules — How the Translator Maps Input to Output

These rules define how a recipe becomes an allergen matrix. The output shape is defined in `reference/output-schema.md`. The only source for connecting an ingredient to an allergen is `reference/lookup-table.md`.

The governing constraint: **every value in the output traces to the input, the lookup table, or fixed text in the schema.** When none of those supplies a value, the output says so.

---

## 1. Define the Input

1. The input is everything the user provides, whether pasted into the message or attached as a file, except an explicit request to the translator ("run this," "translate this recipe"). Source, attribution, and note lines are part of the input and are numbered like any other line.
2. The entire input is treated as one recipe. Section headings inside it ("For the dressing:") are part of that recipe.
3. The translator does not use any recipe, ingredient list, or brand information that is not in the input or the lookup table.

---

## 2. Number the Lines (Section 5)

1. Each line break in the input starts a new line. Blank lines are skipped and not numbered.
2. Lines are numbered `L1` through `Ln` in input order and reproduced verbatim: same spelling, capitalization, punctuation, quantities, and units. Misspellings are not corrected.
3. Instruction lines are numbered the same way as ingredient lines.
4. A long line is never split, and separate lines are never merged.

---

## 3. Fill the Header (Section 0)

1. **RECIPE:** the dish's name, taken from the input, never written by the translator.

   **Title candidates** are lines that come before the first ingredient line and meet all of these tests:

   - **No quantity.** Excludes "4 Servings," "16 items," "3.30(106)."
   - **Not a generic label:** "Ingredients," "Ingredient list," "What you need," "Directions," "Instructions," "Method," "How it's done," "Notes," "Tips," "Serves," "Nutrition."
   - **Not a page control.** Excludes any line beginning with Save, Print, Add, Log, Share, Jump, Rate, Pin, Copy, Browse, Keep, Learn, Shop, Subscribe, Follow, Sign, Skip, or Search — these are site buttons and links, not dish names.
   - **Not prose.** A title is short: 60 characters or fewer, and it does not end in sentence punctuation (`.`, `!`, `?`). Excludes a recipe's description or headnote.
   - **Not inside a non-recipe section** (rule 5.9).

   - No candidate → `Not in source`.
   - One candidate → that line's text, with its line number: `Glazed Meatloaf (L1)`.
   - More than one candidate → the candidate **nearest the first ingredient line**, with its line number, followed by `— unconfirmed; candidates: <the other candidate L-numbers, up to three, nearest first>`. Example: `Glazed Meatloaf (L10) — unconfirmed; candidates: L7, L6`.
   - If the user's message names the recipe outside the pasted input, that name is used instead, followed by `(user message)`, and no candidates are listed.

   The line used as RECIPE is the **title**; every other line that labels a section is a **heading**.
2. **SOURCE:** a URL, publication, or attribution stated in the input, followed by its line number in parentheses, or stated in the user's message outside the pasted input, followed by `(user message)`. Otherwise, `Not in source`. A header value is never carried over from anywhere else.
3. **TABLE:** the version stated at the top of `lookup-table.md`.
4. **STATUS:** computed after the matrix (see Section 9 of these rules).

---

## 4. Extract Ingredient Terms

An ingredient term is the verbatim part of a line that names one ingredient.

1. **Strip** quantities, units, and preparation words: e.g., chopped, minced, diced, grated, softened, melted, room-temperature, divided, to taste, for serving, for garnish, optional.
2. **Keep** everything else that names the ingredient, including descriptors (large, red, boneless), qualifiers (fresh, bottled, toasted, refined), free-from or substitute claims (vegan, gluten-free, dairy-free), and brand names. The term is always text that appears in the input.
3. **Split** a line into multiple terms when it names more than one ingredient, including alternatives ("butter or margarine" is two terms).
4. **Optional and garnish ingredients are still terms.** They may be served, so they are matched like any other term.
5. **Term IDs:**
   - A line with exactly one term: the term ID is the line number (`L4`).
   - A line with more than one term: `L<n>.1`, `L<n>.2`, and so on, in order of appearance (`L7.1`, `L7.2`).
6. **Conditional terms.** A term is marked in the Note column of Line Accounting when its line shows it may not be used:
   - `Alternative` — the term is one of two or more ingredients offered in place of each other on the same line ("spaghetti noodles (or thin flat egg noodles)," "butter or margarine").
   - `Optional` — the line marks the ingredient as not required, using "optional," "as desired," "if desired," "if using," or "to garnish."
   - `Serving` — the term is something the dish is served with or on rather than an ingredient of it: "serve with," "serve over," "pour over," "spoon over," "alongside," "for dipping."
   A term that fits more than one of these is marked with the first that applies, in the order `Alternative`, `Optional`, `Serving`. All conditional terms are matched and reported like any other term; the mark never removes an allergen.
7. **Title and heading lines** contain no terms. Ingredient words in a title ("Garlic Butter Shrimp") are not terms. These lines go in the grouped row of Line Accounting (see the schema).

---

## 5. Classify Lines and Handle References

1. **Ingredient lines** are lines that begin with a quantity, or that appear under an ingredients heading ("Ingredients," "What you need," "For the glaze:") before the next heading. Terms on ingredient lines are extracted under Section 4 of these rules.
2. **All other lines** (instructions, descriptions, notes, and any other text in the input) are checked for ingredient mentions. A mention is a **reference**, not a new term, if it matches the name, or a shortened or plural form of the name, of an ingredient on **any** ingredient line in the input, or if it names a section heading in the input ("tangy glaze" when the input has "For the Glaze:"). A mention that could refer to more than one listed ingredient references all of them ("pepper" when both "bell pepper" and "black pepper" are listed).
3. A mention of the ingredient list as a whole ("all ingredients," "the rest of the ingredients") references every ingredient line in the input.
4. A mention on a non-ingredient line that matches no ingredient line and no heading is a **new term** and is matched like any other term ("fry in peanut oil" when no oil was listed).
5. A non-ingredient line that contains references but no new terms receives its own `NO-INGREDIENT` row with the note `References <L-numbers>`. A line with no terms and no references goes in the grouped row of Line Accounting (see the schema).
6. A non-ingredient line with new terms receives one row per new term. If it also contains references, the first term's note includes `References <L-numbers>`.
7. Mentions in steps about handwashing, cleaning, or equipment ("wash hands with soap and water," "grease a baking sheet") are not ingredient mentions.
8. **Source and attribution lines.** An ingredient name inside a source, attribution, or publication-title line ("USDA's Collection of Nonfat Dry Milk Recetas") is not an ingredient mention.
9. **Non-recipe sections.** Lines that fall under a heading belonging to the surrounding page rather than the recipe are not ingredient mentions and produce no terms: "You might also like," "Related recipes," "More recipes," "Reviews," "Comments," "Ratings," "Nutrition Facts," "Nutrition Information," "Portions per serving," "Advertisement," "Shop," "Newsletter," and "Learn more about." A section ends at the next heading. These lines are still numbered in the Numbered Source and still appear in the grouped row of Line Accounting, so nothing is dropped from the accounting — only from the matrix.
10. **Writing L-number lists.** In a `References` note and in the grouped row, three or more consecutive numbers are written as a range with an en dash (`L3–L14`); two consecutive numbers or isolated numbers are listed separately, separated by `, `. Ranges never cross a term ID with a decimal point: a line split into terms is listed as `L8.1, L8.2`.

---

## 6. Match Each Term to a Table Row

1. **Matching is case-insensitive.** Singular and plural forms match each other. US and UK spellings match each other (sulfite/sulphite, yogurt/yoghurt).
2. **Most specific match wins.** When a term could match more than one row, the row whose listed phrase covers the most of the term wins:
   - "peanut butter" → T-031, not butter (T-001)
   - "almond milk" → T-025, not milk (T-002)
   - "coconut milk" → T-081, not milk (T-002)
   - "cream cheese" → T-004, not cream (T-003)
   - "rye bread" → T-044, not bread (T-037)
   - "fresh lemon juice" → T-077, not lemon juice (T-067)
3. **Descriptors are ignored for matching** when no row lists them. Descriptors are words that state size, color, variety, cut, freshness, or processing without naming a different food: "large eggs" matches T-008, "red onion" matches T-074, "fresh salmon" matches T-013, "refined peanut oil" matches T-032. When a row lists the qualified phrase ("fresh lemon juice," "toasted sesame oil"), that row wins under rule 2.
4. **A word that names a different food or product is not a descriptor.** "Ice cream," "cream of mushroom soup," "honey mustard," "rice wine," and "chicken sausage" do not match cream, mushroom, mustard, wine, or chicken. They are `UNRESOLVED` unless a row lists them.
5. **Source modifiers on T-061 (stock).** "Chicken stock," "fish broth," and "vegetable bouillon" match T-061. The stock term is the stock word with its descriptors but without the source modifier ("low-sodium chicken bouillon" → `chicken` and `low-sodium bouillon`). The source modifier is recorded as its own term, numbered in order of appearance under rule 4.5 ("chicken stock" → `L4.1` chicken, `L4.2` stock), and matched separately, so "fish stock" produces a Fish term. A source modifier that matches no row ("vegetable") is not recorded as a term.
6. **Category rows** ("any named cheese," "any named fish," "any single named herb," "any single named spice," "any named fresh fruit or vegetable") match only a term that is a commonly known member of that category. If membership is uncertain, the term is `UNRESOLVED`.
7. **Free-from and substitute claims** ("vegan butter," "gluten-free panko," "dairy-free cheese") block matching to the unqualified row. They are `UNRESOLVED` unless a row names the qualified version.
8. **Brand names:** a brand name alongside a generic ingredient name ("Lea & Perrins Worcestershire sauce") matches the generic row. A brand name with no generic ingredient name is `UNRESOLVED`.
9. **No match:** a term that matches no row is `UNRESOLVED`. The translator does not use general knowledge to assign an allergen to it, even when the answer seems obvious.

---

## 7. Assign Term Status (Section 4)

| Condition | Status |
|---|---|
| Matched row has an allergen in Assigns | `MATCHED-ALLERGEN` |
| Matched row has no allergen in Assigns, but has one in Possible | `MATCHED-POSSIBLE` |
| Matched row has Assigns `None` and nothing in Possible | `MATCHED-NONE` |
| No row matched | `UNRESOLVED` |

A row with allergens in both Assigns and Possible produces `MATCHED-ALLERGEN`. Its possible allergens still count toward Undetermined cells, and the term still enters the Review Queue.

The Allergens column lists assigned allergens first, then possible allergens tagged `(possible)`, separated by `, `. The Note column copies the row's Reason / note text only when it states an out-of-scope allergen or the reason for a possible flag.

---

## 8. Compute Matrix Cells (Section 2)

For each of the 15 allergens, in schema order:

1. If any term's matched row lists the allergen in **Assigns** → `Present`.
2. Otherwise, if any term's matched row lists it in **Possible** → `Undetermined`.
3. Otherwise → `Not found in listed ingredients`.

The Source column lists every term that contributed, in term order, using the evidence format in the schema. A Present cell also lists terms that flag the allergen as possible, tagged `(possible)`.

---

## 9. Compute the Status Line

1. If the input produced no ingredient terms → `STATUS: NO INGREDIENTS FOUND`.
2. Otherwise, if any term is `UNRESOLVED` → `STATUS: PROVISIONAL`, listing the count and term IDs.
3. Otherwise → `STATUS: RESOLVED`.

When the status is PROVISIONAL, every Not found cell's Source column lists the unresolved term IDs.

---

## 10. Build the Review Queue (Section 3)

Every `UNRESOLVED` term, and every term whose matched row has entries in Possible, enters the queue in term order. The action is chosen by the first condition that applies.

**For any queued term:** if the input marks it as made in-house ("house," "homemade," "our," or a sub-recipe elsewhere in the input) → `Check house recipe`.

**Otherwise, for UNRESOLVED terms:**

| Condition | Action |
|---|---|
| The term is too general to identify an ingredient ("oil," "sauce," "seasoning salt") | `Clarify ingredient` |
| All other unresolved terms | `Check supplier label` |

**Otherwise, for terms with possible allergens:**

| Condition | Action |
|---|---|
| The row's reason includes "not stated" | `Clarify ingredient` |
| The row's reason includes "brand," "brands," "formulation," or "Brand-dependent" | `Confirm brand` |
| All other rows with possible allergens | `Check supplier label` |

---

## 11. Build At a Glance (Section 1)

At a Glance is built last, from the finished Allergen Matrix and Review Queue, following the format in the schema. It restates those sections; it adds nothing.

1. **Present:** each allergen with a Present cell, in matrix order, followed in parentheses by the distinct terms whose matched rows **assign** it, exactly as written in the input. A term marked `Alternative`, `Optional`, or `Serving` in Line Accounting carries an asterisk after it. Terms that only flag the allergen as possible are not listed here. Duplicate terms are listed once ("yellow mustard" from two lines appears once).
2. **Undetermined:** each allergen with an Undetermined cell, followed in parentheses by the distinct terms that flag it as possible.
3. A line with no allergens reads `—`.
4. **Conditional note:** if any term in the Present or Undetermined lines carries an asterisk, the fixed footnote line follows them, reproduced exactly, beginning with the words `Note (*):`:

```
Note (*): Listed as optional, as an alternative, or as something the dish is served with. May not be used in every preparation — confirm which ingredients were used.
```

   If no term carries an asterisk, the footnote line is omitted.
5. **Needs review:** the count of Review Queue items, then one line per distinct term and action, in queue order. Items with the same term (ignoring capitalization) and the same action share a line listing all their term IDs. If the queue is empty, `Needs review: —`.

---

## 12. Scope Statement (Section 6)

Section 6 reproduces the fixed Scope Statement from the schema exactly.

The translator does not search the web or use any source other than the input and `reference/`. Adding ingredients to the lookup table is a separate, human-reviewed process outside the translator.

---

## 13. Never Add

The translator never:

- Assigns or flags an allergen that is not in a matched table row.
- Writes a recipe name, source, quantity, brand, or ingredient that is not in the input.
- Corrects, normalizes, or completes the spelling of any input text.
- States or implies that a dish is free of any allergen.
- Uses any cell value, status, action, or note wording that is not defined in the schema.
- Adds advice, substitutions, cross-contact warnings, commentary, or a summary. The Scope Statement is the only caveat.
- Omits a section, reorders sections, or changes a heading, including when a section is empty.
- Adds any text before the `RECIPE:` line or after the Scope Statement.
- Refers to, compares against, or explains differences from any previous run, earlier output, or earlier version of these files. Each run is independent.

---

## 14. Check Before Returning

Before returning the output, verify:

1. Every numbered line appears in Line Accounting exactly once, either in its own row or in the grouped row.
2. Every term has exactly one status.
3. Every Present and Undetermined cell cites at least one term ID and table row.
4. Every term ID cited in the matrix exists in Line Accounting.
5. The STATUS line, the unresolved count, and the `open:` lists agree.
6. Every term with status `UNRESOLVED` or with possible allergens appears in the Review Queue, and no other term does.
7. All seven sections (0–6) appear, in order, with schema headings.
8. The output holds up against the Blank Shape in the schema.
9. The output begins with the `RECIPE:` line and ends with the Scope Statement, with nothing before or after.
10. Every allergen and term in At a Glance matches the Allergen Matrix, and the Needs review count matches the Review Queue.
11. If any term in At a Glance carries an asterisk, the `Note (*):` line is present directly below the Undetermined line.

If any check fails, correct the output before returning it.
