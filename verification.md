# Verification — Tracing One Output Claim by Claim

This file does what a reader should be able to do for themselves: take one output and check every claim in it against the input, the lookup table, or the schema. If a claim traces to none of those three, the translator invented it and the entry fails.

**Output verified:** Glazed Meatloaf, lookup table 1.4, reproduced in full in `examples.md` (Example 1).
**Input verified against:** `inputs/glazed-meatloaf.txt`, 35 lines.
**Method:** every value in the output was checked by hand against the numbered input line it cites, the table row it cites, or the fixed text in `reference/output-schema.md`.

**Result: 0 invented claims.**

---

## 1. Header (4 claims)

| Output | Traces to | Checks out |
|---|---|---|
| `RECIPE: Glazed Meatloaf (L1)` | Input L1, verbatim | Yes |
| `SOURCE: USDA Center for Nutrition Policy and Promotion, MyPlate Kitchen, https://www.myplate.gov/recipes/glazed-meatloaf (L35)` | Input L35, verbatim | Yes |
| `TABLE: lookup-table 1.4` | Version line at the top of `reference/lookup-table.md` | Yes |
| `STATUS: RESOLVED — all ingredient terms matched.` | Schema fixed text; no term has status `UNRESOLVED` in Line Accounting | Yes |

---

## 2. Positive allergen claims (5 claims)

Each cell cites a term ID and a table row. Both halves were checked: that the input line says what the term says, and that the table row assigns or flags what the cell claims.

| Cell | Cites | Input line says | Table row says | Checks out |
|---|---|---|---|---|
| Egg: **Present** | L14 → T-008 | "1 large egg" | T-008 assigns Egg to "egg, eggs, … whole egg" | Yes |
| Other gluten cereals: **Present** | L15 → T-045 | "1/2 cup rolled oats" | T-045 assigns Other gluten cereals to "oats, rolled oats, …" | Yes |
| Mustard: **Present** | L10 → T-057; L18 → T-057 | "1 tablespoon yellow mustard"; "1 teaspoon yellow mustard" | T-057 assigns Mustard to "mustard, yellow mustard, …" | Yes |
| Soybeans: **Undetermined** | L3 → T-084 (possible) | "1 teaspoon vegetable oil" | T-084 lists Soybeans under Possible, reason "Oil source not stated" | Yes |
| Sulphites: **Undetermined** | L10 → T-057 (possible); L18 → T-057 (possible) | the two mustard lines | T-057 lists Sulphites under Possible, reason "Some prepared mustards contain wine or sulphite preservatives" | Yes |

**The cell values follow the rules' precedence.** Soybeans is Undetermined rather than Present because no term's row *assigns* it. Mustard is Present because T-057 assigns it, and the same row's possible Sulphites flag lands on the Sulphites row instead.

---

## 3. Negative allergen claims (10 claims)

Ten rows read `Not found in listed ingredients` with `—` in the Source column: Milk, Fish, Crustacean shellfish, Molluscs, Tree nuts, Peanuts, Wheat, Sesame, Celery, and Lupin. (Soybeans and Sulphites are Undetermined, not negative, and are covered in section 2.)

Each of these is a claim about absence, so it was checked the other way around: every one of the 16 terms in Line Accounting was looked up in the table to confirm that no row assigns or flags the allergen in question. Terms and rows: vegetable oil (T-084), onion (T-074), green bell pepper (T-077), garlic (T-074), dried thyme (T-075), tomato paste (T-085, twice), water (T-072), yellow mustard (T-057, twice), salt (T-070), black pepper (T-070), ground beef (T-078), turkey (T-078), large egg (T-008), rolled oats (T-045).

None of those rows names Milk, Fish, Crustacean shellfish, Molluscs, Tree nuts, Peanuts, Wheat, Sesame, Celery, or Lupin, so each negative is supported.

**These rows show `—` rather than `· open:` because the status is RESOLVED.** No term is unresolved, so no negative is provisional. Compare Example 2, where three unresolved terms put `· open: L4, L9, L26.1` on every negative row.

---

## 4. At a Glance (6 claims)

At a Glance is derived, so it was checked against the matrix rather than the input.

| Output | Derived from | Checks out |
|---|---|---|
| `Present: Egg (large egg) · Other gluten cereals (rolled oats) · Mustard (yellow mustard)` | The three Present cells, in matrix order, with the terms whose rows assign them | Yes |
| `Undetermined: Soybeans (vegetable oil) · Sulphites (yellow mustard)` | The two Undetermined cells and the terms that flag them | Yes |
| "yellow mustard" appears once, not twice | Rules: duplicate terms are listed once | Yes |
| No asterisk on any term | The only conditional terms are ground beef and turkey (`Alternative`), and neither appears in these lines | Yes |
| No footnote line | No asterisk appears above it | Yes |
| `Needs review: 3 item(s)` with 2 lines | 3 review queue items; the two mustard items share a line because term and action match | Yes |

---

## 5. Review queue (3 claims)

| Item | Action | Rule that sets it | Checks out |
|---|---|---|---|
| L3 vegetable oil | `Clarify ingredient` | T-084's reason includes "not stated" | Yes |
| L10 yellow mustard | `Check supplier label` | T-057's reason includes neither "not stated" nor "brand," so it falls to the default | Yes |
| L18 yellow mustard | `Check supplier label` | Same row | Yes |

Every term whose row lists a possible allergen appears here, and no other term does: L3 (T-084), L10 and L18 (T-057). No term is unresolved.

---

## 6. Line accounting (35 lines)

| Check | Result |
|---|---|
| Every input line appears exactly once, in its own row or the grouped row | 35 of 35 |
| Own rows | 16 term rows + 9 reference rows = 25 |
| Grouped row | `L1, L2, L16, L19, L20, L25, L29, L30, L33–L35` = 10 lines |
| Every term is a verbatim substring of its line | 16 of 16 |
| Every reference note points to a line that names that ingredient | 9 of 9 |
| Every table row cited exists in lookup table 1.4 | 16 of 16 |

**Two accounting details worth checking by hand:**

- **L27** ("put the ground beef, 4 tablespoons rolled oats and the egg…") notes `References L4, L13.1, L14, L15`. The oats are a reference, not a second term, so the recipe's 1/2 cup and this step's 4 tablespoons are not counted as two oat ingredients.
- **L31** ("To make the glaze: Put the tomato paste and mustard…") notes `References L8, L10, L16–L18`, which includes L16, the "For the Glaze:" heading. The phrase "the glaze" points at a heading in the input rather than becoming a new ingredient.

---

## 7. Numbered source (35 lines)

Each of the 35 numbered lines was compared character by character with `inputs/glazed-meatloaf.txt`. All 35 match, including the degree symbol in L20 ("325 °F") and the unusual loaf dimensions in L30 ("8x4 inches"). No line was split, merged, reordered, or corrected.

---

## 8. Shape (schema compliance)

| Check | Result |
|---|---|
| All seven sections present, in schema order | Yes |
| Section headings match the schema | Yes |
| Matrix has 15 rows in schema order | Yes |
| Only the three permitted cell values appear | Yes |
| Only the five permitted statuses appear | Yes |
| Only the four permitted review actions appear | Yes |
| Scope statement matches the schema's fixed text word for word | Yes |
| Nothing before `RECIPE:` or after the scope statement | Yes |

---

## 9. Reproducibility

| Run | Table | Input | Result |
|---|---|---|---|
| 1 | 1.1 | Clean file | Baseline |
| 2 | 1.1 | Clean file | Identical to run 1 |
| 3 | 1.2 | Clean file | Identical allergen results; differences confined to the table version line, the new `Alternative` notes, and the new L-number range format |
| 4 | 1.3 | Clean file | Identical to run 3 apart from the table version line |
| 5 | 1.3 | Clean file | Identical to run 4, with `examples.md` loaded in the project |
| 6 | 1.3 | Clean file | Identical to run 5, after the non-recipe-section and title-candidate rules were added |
| 7 | 1.3 | 172-line raw page paste | Same three Present allergens, same two Undetermined, same three review items; only the line numbers and `SOURCE: Not in source` differ (Example 5) |
| 8 | 1.4 | Clean file | Identical to run 6 apart from the table version line |
| 9 | 1.4 | 172-line raw page paste | Identical to run 7 apart from the table version line |

Each run was made in a fresh chat with no memory of the others. The shape and every allergen result held across all nine.

Run 4 doubles as a control on the table itself: version 1.3 added five rows (tomato juice, canned and stewed tomatoes, croutons, sausage), none of which appear in this recipe, and nothing in the output moved. Run 7 is the harder control: the same dish pasted straight out of a browser, 172 lines instead of 35, carrying site buttons, a food-group panel, an 86-line nutrition panel, and related-recipe titles naming Cheese, Tuna, and Pasta. Milk, Fish, and Wheat all came back `Not found in listed ingredients`, and every one of the 172 lines was accounted for once. Across the five worked examples, thirteen runs produced the same seven sections with no shape drift, covering both `RESOLVED` and `PROVISIONAL` statuses, an empty review queue (`None.`), and a clean and a cluttered version of the same input.

---

## 10. Edge cases

Four inputs were written to fire rules that no real recipe had reached. All four were run on table 1.3 or 1.4 in fresh chats.

| Input | What it tested | Result |
|---|---|---|
| Grilled chicken with "our house chimichurri" | The `Check house recipe` action, and a dish whose allergens live in an unwritten sauce | The sauce is `UNRESOLVED` with `Check house recipe`; `Present: —`, `Undetermined: —`, and all 15 rows carry `· open:`. Nothing is cleared on the strength of a sauce nobody wrote down. |
| Baked cod with "gluten-free panko" and "vegan butter" | Whether free-from claims borrow the unqualified rows | Both `UNRESOLVED`; Wheat and Milk come back `Not found` with `· open:`, so the output neither asserts an allergen the recipe denies nor clears one it cannot verify. Fish Present from the cod. |
| Marinade with "Lea & Perrins Worcestershire sauce," "Grey Poupon Dijon mustard," and "Old Bay" | Brand-name handling, and the `Confirm brand` action | The two branded products matched their generic rows (T-017, T-057) with the brand kept in the term; "Old Bay" is a brand with no generic ingredient name and is `UNRESOLVED`. `Confirm brand` fired on the Worcestershire. |
| Quarterly safety meeting notes (not a recipe) | `STATUS: NO INGREDIENTS FOUND` | Status correct, all 15 rows `Not found` with plain `—`, `Review Queue: None.`, one grouped row covering every line, all seven sections present. No ingredient was invented from a document that mentions "kitchen management." |

All four review actions — `Check supplier label`, `Confirm brand`, `Check house recipe`, `Clarify ingredient` — have now been produced by a real run.

The marinade test also produced a rule change rather than a defect: "Pour over steak" made steak a term, which is true to the input but presents an accompaniment as an ingredient. Terms from "serve with," "pour over," and similar phrases are now marked `Serving` in Line Accounting and carry the At a Glance asterisk, so they stay visible without being counted as part of the dish.

## 11. Where the rules were wrong, and how it showed

Three defects were found by running inputs rather than by reading the files, and each one is worth knowing about because of how it surfaced:

- **A description sentence read as an ingredient list.** An early page paste treated the recipe's one-line summary as ingredients, producing duplicate mustard and oats terms and a phantom "tangy glaze." Fixed by matching mentions against ingredient lines anywhere in the input, not only earlier ones.
- **A site button read as the recipe name.** After the title rule was changed to prefer the candidate nearest the ingredient list, a page paste returned `RECIPE: Save to Pinterest (L8) — unconfirmed`. The unconfirmed flag meant it was visible rather than silent, and the fix was to exclude page controls and prose from title candidates.
- **A footnote marker disappeared.** The At a Glance footnote began with `* `, which a markdown renderer read as a bullet and swallowed, leaving asterisked terms pointing at nothing. Rewritten to begin `Note (*):`, which survives both the model and the renderer.
- **A related-recipes list was a live hazard.** Before non-recipe sections were defined, a page footer naming "Cheese," "Tuna," and "Pasta" would have put Milk, Fish, and Wheat on a meatloaf. This is the one defect that could have produced a wrong allergen result, and it was found by testing the raw paste rather than a tidy file.

In none of these cases did the translator assert something the input did not contain: the bad results were traceable to real lines, which is how each defect was spotted and fixed.

## 12. What this verification does not prove

- **It does not prove the table's contents are right.** It proves the output faithfully reflects the table. Whether T-045 should assign Other gluten cereals to oats is a claim about food regulation, sourced in the table itself and open to challenge on its own terms.
- **It does not prove the dish is safe for anyone.** Ten negative rows mean ten allergens that the listed ingredients did not indicate, which is not the same as a free-from claim. The scope statement says so in the output itself.
- **It does not cover every allergen row.** Across five worked examples, ten of the fifteen rows have been exercised as Present or Undetermined: Milk, Egg, Molluscs, Tree nuts, Wheat, Other gluten cereals, Soybeans, Mustard, Celery, and Sulphites. The remaining five — Fish, Crustacean shellfish, Peanuts, Sesame, and Lupin — have only ever been reported as negatives.

---

## Verify it yourself

1. Set up the project as described in `README.md`.
2. Open a new chat, attach `inputs/glazed-meatloaf.txt`, and send "Translate this recipe."
3. Compare the output with Example 1 in `examples.md`.
4. Pick any claim in it and trace it: the term ID points to a line in the Numbered Source, and the `T-` number points to a row in `reference/lookup-table.md`.
5. For a harder test, feed it a recipe with an ingredient the table does not cover (Example 2's "hot pepper sauce" is one). The status should turn PROVISIONAL, every negative should carry `· open:`, and the unknown ingredient should appear in the review queue instead of being resolved.
6. For the hardest test, open any recipe page in a browser, select the whole page, and paste it. Compare the result with the clean file for the same dish. `inputs/glazed-meatloaf-page-paste.txt` and Example 5 are that comparison for the Meatloaf.
