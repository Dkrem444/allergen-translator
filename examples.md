# Examples — Recipe → Allergen Matrix

Each example is a real input and the output the translator returned for it, run in a fresh chat with lookup table version 1.4. Together they show the contract holding: the same shape every time, every value traced to a line in the input or a row in the table, and every gap marked rather than filled.

| # | Recipe | What it tests | Status |
|---|---|---|---|
| 1 | Glazed Meatloaf | Clean input, EU-only allergens, a sub-recipe heading, alternatives on one line, instructions that only refer back | Verified on 1.4 |
| 2 | Veggie Chow Mein | A stock with a source modifier, an unqualified "oil," ingredients that appear only in the directions and notes | Verified on 1.4 |
| 3 | Manhattan Clam Chowder | Molluscs, processed tomato products, a misspelling that must be preserved | Verified on 1.4 |
| 4 | Banana Walnut Oatmeal | Milk and tree nuts, a substitution offered outside the ingredient list, an empty review queue | Verified on 1.4 |
| 5 | Glazed Meatloaf (raw page paste) | 172 lines of web page clutter: site buttons, nutrition panel, related-recipe titles naming other allergens | Verified on 1.4 |

---

## Example 1 — Glazed Meatloaf

**Source:** USDA Center for Nutrition Policy and Promotion, MyPlate Kitchen. A federal work in the public domain.

### Input

```text
Glazed Meatloaf

Ingredients
1 teaspoon vegetable oil
1 onion, chopped
1/2 green bell pepper, cored and diced
2 cloves garlic, peeled and diced
1 teaspoon dried thyme
2 tablespoons tomato paste
1/2 cup water
1 tablespoon yellow mustard
1/2 teaspoon salt
1/4 teaspoon black pepper
1 pound ground beef or turkey
1 large egg
1/2 cup rolled oats
For the Glaze:
1 teaspoon tomato paste
1 teaspoon yellow mustard

Directions
1. Preheat the oven to 325 °F. Lightly grease a baking sheet.
2. Put a skillet over medium heat and when it is hot, add the oil.
3. Add the onion, bell pepper, garlic, and thyme and cook about 10 minutes until golden.
4. Lower the heat and add the water and tomato paste.
5. Cook about 10 minutes until the onion is soft and most of the liquid has been absorbed.
6. Set aside to cool.
7. Add the mustard, salt and pepper.
8. While the onion mixture is cool, put the ground beef, 4 tablespoons rolled oats and the egg in the bowl and gently mix, by hand.
9. Add the cooled onion mixture and gently mix again until everything is thoroughly incorporated.
10. Do not over mix.
11. Form the mixture into a loaf about 8x4 inches and put on the prepared baking sheet.
12. To make the glaze: Put the tomato paste and mustard in a small bowl and mix well.
13. Rub on top of the meatloaf and sprinkle with the remaining 2 tablespoons rolled oats.
14. Put in the oven to cook about 1 hour until deep brown and cooked throughout.
15. Serve right away or cover and refrigerate up to 2 days.

Source: USDA Center for Nutrition Policy and Promotion, MyPlate Kitchen, https://www.myplate.gov/recipes/glazed-meatloaf
```

### Output

```text
RECIPE: Glazed Meatloaf (L1)
SOURCE: USDA Center for Nutrition Policy and Promotion, MyPlate Kitchen, https://www.myplate.gov/recipes/glazed-meatloaf (L35)
TABLE: lookup-table 1.4
STATUS: RESOLVED — all ingredient terms matched.

## At a Glance
Present: Egg (large egg) · Other gluten cereals (rolled oats) · Mustard (yellow mustard)
Undetermined: Soybeans (vegetable oil) · Sulphites (yellow mustard)
Needs review: 3 item(s)
- vegetable oil (L3) — Clarify ingredient
- yellow mustard (L10, L18) — Check supplier label

## Allergen Matrix
| Allergen | Cell | Source |
|---|---|---|
| Milk | Not found in listed ingredients | — |
| Egg | Present | L14 → T-008 |
| Fish | Not found in listed ingredients | — |
| Crustacean shellfish | Not found in listed ingredients | — |
| Molluscs | Not found in listed ingredients | — |
| Tree nuts | Not found in listed ingredients | — |
| Peanuts | Not found in listed ingredients | — |
| Wheat | Not found in listed ingredients | — |
| Other gluten cereals | Present | L15 → T-045 |
| Soybeans | Undetermined | L3 → T-084 (possible) |
| Sesame | Not found in listed ingredients | — |
| Mustard | Present | L10 → T-057; L18 → T-057 |
| Celery | Not found in listed ingredients | — |
| Lupin | Not found in listed ingredients | — |
| Sulphites | Undetermined | L10 → T-057 (possible); L18 → T-057 (possible) |

## Review Queue
1. L3 — "vegetable oil" — Clarify ingredient
2. L10 — "yellow mustard" — Check supplier label
3. L18 — "yellow mustard" — Check supplier label

## Line Accounting
| ID | Term | Status | Table row | Allergens | Note |
|---|---|---|---|---|---|
| L3 | vegetable oil | MATCHED-POSSIBLE | T-084 | Soybeans (possible) | Oil source not stated; vegetable oil is often soybean oil. |
| L4 | onion | MATCHED-NONE | T-074 | — | — |
| L5 | green bell pepper | MATCHED-NONE | T-077 | — | — |
| L6 | garlic | MATCHED-NONE | T-074 | — | — |
| L7 | dried thyme | MATCHED-NONE | T-075 | — | — |
| L8 | tomato paste | MATCHED-NONE | T-085 | — | — |
| L9 | water | MATCHED-NONE | T-072 | — | — |
| L10 | yellow mustard | MATCHED-ALLERGEN | T-057 | Mustard, Sulphites (possible) | Some prepared mustards contain wine or sulphite preservatives. |
| L11 | salt | MATCHED-NONE | T-070 | — | — |
| L12 | black pepper | MATCHED-NONE | T-070 | — | — |
| L13.1 | ground beef | MATCHED-NONE | T-078 | — | Alternative |
| L13.2 | turkey | MATCHED-NONE | T-078 | — | Alternative |
| L14 | large egg | MATCHED-ALLERGEN | T-008 | Egg | — |
| L15 | rolled oats | MATCHED-ALLERGEN | T-045 | Other gluten cereals | — |
| L17 | tomato paste | MATCHED-NONE | T-085 | — | — |
| L18 | yellow mustard | MATCHED-ALLERGEN | T-057 | Mustard, Sulphites (possible) | Some prepared mustards contain wine or sulphite preservatives. |
| L21 | — | NO-INGREDIENT | — | — | References L3 |
| L22 | — | NO-INGREDIENT | — | — | References L4–L7 |
| L23 | — | NO-INGREDIENT | — | — | References L8, L9, L17 |
| L24 | — | NO-INGREDIENT | — | — | References L4 |
| L26 | — | NO-INGREDIENT | — | — | References L5, L10–L12, L18 |
| L27 | — | NO-INGREDIENT | — | — | References L4, L13.1, L14, L15 |
| L28 | — | NO-INGREDIENT | — | — | References L4 |
| L31 | — | NO-INGREDIENT | — | — | References L8, L10, L16–L18 |
| L32 | — | NO-INGREDIENT | — | — | References L15 |
| L1, L2, L16, L19, L20, L25, L29, L30, L33–L35 | — | NO-INGREDIENT | — | — | No ingredient mentions |

## Numbered Source
L1 Glazed Meatloaf
L2 Ingredients
L3 1 teaspoon vegetable oil
L4 1 onion, chopped
L5 1/2 green bell pepper, cored and diced
L6 2 cloves garlic, peeled and diced
L7 1 teaspoon dried thyme
L8 2 tablespoons tomato paste
L9 1/2 cup water
L10 1 tablespoon yellow mustard
L11 1/2 teaspoon salt
L12 1/4 teaspoon black pepper
L13 1 pound ground beef or turkey
L14 1 large egg
L15 1/2 cup rolled oats
L16 For the Glaze:
L17 1 teaspoon tomato paste
L18 1 teaspoon yellow mustard
L19 Directions
L20 1. Preheat the oven to 325 °F. Lightly grease a baking sheet.
L21 2. Put a skillet over medium heat and when it is hot, add the oil.
L22 3. Add the onion, bell pepper, garlic, and thyme and cook about 10 minutes until golden.
L23 4. Lower the heat and add the water and tomato paste.
L24 5. Cook about 10 minutes until the onion is soft and most of the liquid has been absorbed.
L25 6. Set aside to cool.
L26 7. Add the mustard, salt and pepper.
L27 8. While the onion mixture is cool, put the ground beef, 4 tablespoons rolled oats and the egg in the bowl and gently mix, by hand.
L28 9. Add the cooled onion mixture and gently mix again until everything is thoroughly incorporated.
L29 10. Do not over mix.
L30 11. Form the mixture into a loaf about 8x4 inches and put on the prepared baking sheet.
L31 12. To make the glaze: Put the tomato paste and mustard in a small bowl and mix well.
L32 13. Rub on top of the meatloaf and sprinkle with the remaining 2 tablespoons rolled oats.
L33 14. Put in the oven to cook about 1 hour until deep brown and cooked throughout.
L34 15. Serve right away or cover and refrigerate up to 2 days.
L35 Source: USDA Center for Nutrition Policy and Promotion, MyPlate Kitchen, https://www.myplate.gov/recipes/glazed-meatloaf

## Scope Statement
This matrix reports allergens identified in the listed ingredients using the stated lookup table version. It covers the US FDA major food allergens and the EU/UK declarable allergens only. It does not assess cross-contact, supplier formulation changes, sulphite concentration, or the preparation environment. It is not an allergen certification. Undetermined cells and unresolved terms require human review before this information is shared with guests.
```

### What this example shows

- **EU-only allergens are caught.** Rolled oats (L15) produce Other gluten cereals and yellow mustard (L10, L18) produces Mustard. Neither is a US major allergen, but both are on the EU/UK list.
- **"Undetermined" is not "Present."** Vegetable oil (L3) does not state its source, so soybeans are flagged for review rather than assigned.
- **Alternatives become separate terms.** "Ground beef or turkey" (L13) is two terms, L13.1 and L13.2.
- **A sub-recipe heading is a reference target.** Step 12 (L31) says "make the glaze," which points back to the "For the Glaze:" heading (L16) instead of becoming a new term.
- **Instructions don't double-count.** "4 tablespoons rolled oats" in step 8 (L27) refers back to L15 rather than adding a second oats term.
- **An ambiguous mention points to every match.** "Pepper" in step 7 (L26) refers to both the bell pepper (L5) and the black pepper (L12).
- **Every line is accounted for once.** 25 own rows plus 1 grouped row cover all 35 lines.
- **Alternatives are marked.** "Ground beef or turkey" produces two terms, each noted `Alternative`. Neither carries an allergen here, so no asterisk appears in At a Glance.
- **Consistency:** three separate runs on this input, each in a fresh chat, produced identical allergen results.

---

## Example 2 — Veggie Chow Mein

**Source:** University of Illinois Extension Service, published by USDA MyPlate Kitchen.

### Input

```text
Veggie Chow Mein

Ingredients
6 ounces spaghetti noodles (or thin flat egg noodles)
4 teaspoons oil
1 medium onion, finely chopped
2 garlic cloves, finely chopped
1 cup carrots, grated
2 teaspoons low-sodium chicken bouillon
1 teaspoon hot pepper sauce
1 cup broccoli, cut into small pieces
1 cup celery, chopped
1 cup green or red bell peppers, finely chopped
4 teaspoons reduced-sodium soy sauce

Directions
1. Wash hands with soap and water.
2. Prepare noodles according to package directions.
3. Drain and set aside.
4. Cook onion and garlic with oil in a large frying pan for 1 minute over medium/high heat.
5. Add carrot, chicken bouillon, and pepper sauce.
6. Stir.
7. Add broccoli, celery, and bell pepper and continue to stir.
8. Reduce heat to low and add noodles and soy sauce.
9. Mix well over low heat for 3 to 5 minutes.
10. Add salt and pepper to taste.

Notes
Add other vegetables as desired. Top with chives.

Source: University of Illinois Extension Service, published by USDA MyPlate Kitchen, https://www.myplate.gov/recipes/veggie-chow-mein
```

### Output

```text
RECIPE: Veggie Chow Mein (L1)
SOURCE: University of Illinois Extension Service, published by USDA MyPlate Kitchen, https://www.myplate.gov/recipes/veggie-chow-mein (L27)
TABLE: lookup-table 1.4
STATUS: PROVISIONAL — 3 unresolved term(s): L4, L9, L26.1. "Not found" cells are provisional until reviewed.

## At a Glance
Present: Egg (thin flat egg noodles*) · Wheat (spaghetti noodles*, thin flat egg noodles*) · Soybeans (reduced-sodium soy sauce) · Celery (celery)
Undetermined: —
Note (*): Listed as optional, as an alternative, or as something the dish is served with. May not be used in every preparation — confirm which ingredients were used.
Needs review: 6 item(s)
- spaghetti noodles (L3.1) — Check supplier label
- oil (L4) — Clarify ingredient
- low-sodium bouillon (L8.2) — Check supplier label
- hot pepper sauce (L9) — Check supplier label
- reduced-sodium soy sauce (L13) — Check supplier label
- other vegetables (L26.1) — Clarify ingredient

## Allergen Matrix
| Allergen | Cell | Source |
|---|---|---|
| Milk | Not found in listed ingredients | — · open: L4, L9, L26.1 |
| Egg | Present | L3.1 → T-040 (possible); L3.2 → T-011 |
| Fish | Not found in listed ingredients | — · open: L4, L9, L26.1 |
| Crustacean shellfish | Not found in listed ingredients | — · open: L4, L9, L26.1 |
| Molluscs | Not found in listed ingredients | — · open: L4, L9, L26.1 |
| Tree nuts | Not found in listed ingredients | — · open: L4, L9, L26.1 |
| Peanuts | Not found in listed ingredients | — · open: L4, L9, L26.1 |
| Wheat | Present | L3.1 → T-040; L3.2 → T-011; L8.2 → T-061 (possible); L13 → T-047 (possible) |
| Other gluten cereals | Not found in listed ingredients | — · open: L4, L9, L26.1 |
| Soybeans | Present | L8.2 → T-061 (possible); L13 → T-047 |
| Sesame | Not found in listed ingredients | — · open: L4, L9, L26.1 |
| Mustard | Not found in listed ingredients | — · open: L4, L9, L26.1 |
| Celery | Present | L8.2 → T-061 (possible); L11 → T-059 |
| Lupin | Not found in listed ingredients | — · open: L4, L9, L26.1 |
| Sulphites | Not found in listed ingredients | — · open: L4, L9, L26.1 |

## Review Queue
1. L3.1 — "spaghetti noodles" — Check supplier label
2. L4 — "oil" — Clarify ingredient
3. L8.2 — "low-sodium bouillon" — Check supplier label
4. L9 — "hot pepper sauce" — Check supplier label
5. L13 — "reduced-sodium soy sauce" — Check supplier label
6. L26.1 — "other vegetables" — Clarify ingredient

## Line Accounting
| ID | Term | Status | Table row | Allergens | Note |
|---|---|---|---|---|---|
| L3.1 | spaghetti noodles | MATCHED-ALLERGEN | T-040 | Wheat, Egg (possible) | Alternative; Some pasta contains egg. |
| L3.2 | thin flat egg noodles | MATCHED-ALLERGEN | T-011 | Egg, Wheat | Alternative |
| L4 | oil | UNRESOLVED | — | — | — |
| L5 | medium onion | MATCHED-NONE | T-074 | — | — |
| L6 | garlic cloves | MATCHED-NONE | T-074 | — | — |
| L7 | carrots | MATCHED-NONE | T-077 | — | — |
| L8.1 | chicken | MATCHED-NONE | T-078 | — | — |
| L8.2 | low-sodium bouillon | MATCHED-POSSIBLE | T-061 | Celery (possible), Wheat (possible), Soybeans (possible) | Stocks commonly include celery; bouillon products may contain wheat or soy. |
| L9 | hot pepper sauce | UNRESOLVED | — | — | — |
| L10 | broccoli | MATCHED-NONE | T-077 | — | — |
| L11 | celery | MATCHED-ALLERGEN | T-059 | Celery | — |
| L12 | green or red bell peppers | MATCHED-NONE | T-077 | — | — |
| L13 | reduced-sodium soy sauce | MATCHED-ALLERGEN | T-047 | Soybeans, Wheat (possible) | Most soy sauces are brewed with wheat; some are not. |
| L16 | — | NO-INGREDIENT | — | — | References L3.1, L3.2 |
| L18 | — | NO-INGREDIENT | — | — | References L4–L6 |
| L19 | — | NO-INGREDIENT | — | — | References L7, L8.1, L8.2, L9 |
| L21 | — | NO-INGREDIENT | — | — | References L10–L12 |
| L22 | — | NO-INGREDIENT | — | — | References L3.1, L3.2, L13 |
| L24 | salt | MATCHED-NONE | T-070 | — | References L12 |
| L26.1 | other vegetables | UNRESOLVED | — | — | Optional |
| L26.2 | chives | MATCHED-NONE | T-075 | — | — |
| L1, L2, L14, L15, L17, L20, L23, L25, L27 | — | NO-INGREDIENT | — | — | No ingredient mentions |

## Numbered Source
L1 Veggie Chow Mein
L2 Ingredients
L3 6 ounces spaghetti noodles (or thin flat egg noodles)
L4 4 teaspoons oil
L5 1 medium onion, finely chopped
L6 2 garlic cloves, finely chopped
L7 1 cup carrots, grated
L8 2 teaspoons low-sodium chicken bouillon
L9 1 teaspoon hot pepper sauce
L10 1 cup broccoli, cut into small pieces
L11 1 cup celery, chopped
L12 1 cup green or red bell peppers, finely chopped
L13 4 teaspoons reduced-sodium soy sauce
L14 Directions
L15 1. Wash hands with soap and water.
L16 2. Prepare noodles according to package directions.
L17 3. Drain and set aside.
L18 4. Cook onion and garlic with oil in a large frying pan for 1 minute over medium/high heat.
L19 5. Add carrot, chicken bouillon, and pepper sauce.
L20 6. Stir.
L21 7. Add broccoli, celery, and bell pepper and continue to stir.
L22 8. Reduce heat to low and add noodles and soy sauce.
L23 9. Mix well over low heat for 3 to 5 minutes.
L24 10. Add salt and pepper to taste.
L25 Notes
L26 Add other vegetables as desired. Top with chives.
L27 Source: University of Illinois Extension Service, published by USDA MyPlate Kitchen, https://www.myplate.gov/recipes/veggie-chow-mein

## Scope Statement
This matrix reports allergens identified in the listed ingredients using the stated lookup table version. It covers the US FDA major food allergens and the EU/UK declarable allergens only. It does not assess cross-contact, supplier formulation changes, sulphite concentration, or the preparation environment. It is not an allergen certification. Undetermined cells and unresolved terms require human review before this information is shared with guests.
```

### What this example shows

- **Ingredients hidden outside the ingredient list are caught.** Salt appears only in step 10 (L24) and chives only in the Notes (L26.2). A translator reading just the ingredient list would drop both.
- **Alternatives are flagged, not resolved.** The recipe offers spaghetti or egg noodles, so both are terms, both are noted `Alternative`, and both carry an asterisk in At a Glance with the footnote. Egg is reported because one option contains it; the footnote tells the reader to confirm which was used.
- **A stock splits into its source and the stock itself.** "Low-sodium chicken bouillon" (L8) becomes `chicken` (L8.1, no allergen) and `low-sodium bouillon` (L8.2, flagging celery, wheat, and soy). Without the split, a fish or shellfish stock would hide its allergen.
- **Vague ingredients are marked, not guessed.** "Oil" (L4) and "other vegetables" (L26.1) are unresolved and sent to review with `Clarify ingredient`.
- **An ambiguous mention points to every match.** "Pepper" in step 10 refers to the listed bell peppers (L12), recorded in the note on the salt row.
- **Non-food mentions are ignored.** "Wash hands with soap and water" (L15) produces no water term.
- **Assigned beats possible.** Wheat is Present because the noodles assign it; the bouillon and soy sauce, which only flag it, are still cited but do not appear in the At a Glance parentheses.

---

## Example 3 — Manhattan Clam Chowder

**Source:** Eat Smart New York! — Cornell University Cooperative Extension, published by USDA MyPlate Kitchen.

### Input

```text
Manhattan Clam Chowder

Ingredients
1 1/2 cups potatoes, sliced
3 medium celery stalks, washed & sliced
1 large onion, chopped
1/2 cup low-sodium tomato puree
1 1/2 cups carrots, washed and sliced
2 cans (6.5 ounces each) clams, chopped and drained
1 1/2 teaspoons dried thyme
1 bay leaf
2 dashs black peppercorns
1 1/2 cups water
2 cups low-sodium tomato juice
1 tablespoon dried parsley flakes

Directions
1. Wash hands with soap and water.
2. Scrub potatoes well to remove any eyes or blemishes.
3. In a slow cooker, combine all ingredients; stir.
4. Cover and cook on low for 8 to 10 hours or until the vegetables are tender.
5. Remove bay leaf and peppercorns before serving.

Source: Eat Smart New York! - Cornell University Cooperative Extension, published by USDA MyPlate Kitchen, https://www.myplate.gov/recipes/manhattan-clam-chowder
```

### Output

```text
RECIPE: Manhattan Clam Chowder (L1)
SOURCE: Eat Smart New York! - Cornell University Cooperative Extension, published by USDA MyPlate Kitchen, https://www.myplate.gov/recipes/manhattan-clam-chowder (L21)
TABLE: lookup-table 1.4
STATUS: RESOLVED — all ingredient terms matched.

## At a Glance
Present: Molluscs (clams) · Celery (medium celery stalks)
Undetermined: —
Needs review: —

## Allergen Matrix
| Allergen | Cell | Source |
|---|---|---|
| Milk | Not found in listed ingredients | — |
| Egg | Not found in listed ingredients | — |
| Fish | Not found in listed ingredients | — |
| Crustacean shellfish | Not found in listed ingredients | — |
| Molluscs | Present | L8 → T-022 |
| Tree nuts | Not found in listed ingredients | — |
| Peanuts | Not found in listed ingredients | — |
| Wheat | Not found in listed ingredients | — |
| Other gluten cereals | Not found in listed ingredients | — |
| Soybeans | Not found in listed ingredients | — |
| Sesame | Not found in listed ingredients | — |
| Mustard | Not found in listed ingredients | — |
| Celery | Present | L4 → T-059 |
| Lupin | Not found in listed ingredients | — |
| Sulphites | Not found in listed ingredients | — |

## Review Queue
None.

## Line Accounting
| ID | Term | Status | Table row | Allergens | Note |
|---|---|---|---|---|---|
| L3 | potatoes | MATCHED-NONE | T-077 | — | — |
| L4 | medium celery stalks | MATCHED-ALLERGEN | T-059 | Celery | — |
| L5 | large onion | MATCHED-NONE | T-074 | — | — |
| L6 | low-sodium tomato puree | MATCHED-NONE | T-085 | — | — |
| L7 | carrots | MATCHED-NONE | T-077 | — | — |
| L8 | clams | MATCHED-ALLERGEN | T-022 | Molluscs | — |
| L9 | dried thyme | MATCHED-NONE | T-075 | — | — |
| L10 | bay leaf | MATCHED-NONE | T-075 | — | — |
| L11 | black peppercorns | MATCHED-NONE | T-070 | — | — |
| L12 | water | MATCHED-NONE | T-072 | — | — |
| L13 | low-sodium tomato juice | MATCHED-NONE | T-086 | — | — |
| L14 | dried parsley flakes | MATCHED-NONE | T-075 | — | — |
| L17 | — | NO-INGREDIENT | — | — | References L3 |
| L18 | — | NO-INGREDIENT | — | — | References L3–L14 |
| L20 | — | NO-INGREDIENT | — | — | References L10, L11 |
| L1, L2, L15, L16, L19, L21 | — | NO-INGREDIENT | — | — | No ingredient mentions |

## Numbered Source
L1 Manhattan Clam Chowder
L2 Ingredients
L3 1 1/2 cups potatoes, sliced
L4 3 medium celery stalks, washed & sliced
L5 1 large onion, chopped
L6 1/2 cup low-sodium tomato puree
L7 1 1/2 cups carrots, washed and sliced
L8 2 cans (6.5 ounces each) clams, chopped and drained
L9 1 1/2 teaspoons dried thyme
L10 1 bay leaf
L11 2 dashs black peppercorns
L12 1 1/2 cups water
L13 2 cups low-sodium tomato juice
L14 1 tablespoon dried parsley flakes
L15 Directions
L16 1. Wash hands with soap and water.
L17 2. Scrub potatoes well to remove any eyes or blemishes.
L18 3. In a slow cooker, combine all ingredients; stir.
L19 4. Cover and cook on low for 8 to 10 hours or until the vegetables are tender.
L20 5. Remove bay leaf and peppercorns before serving.
L21 Source: Eat Smart New York! - Cornell University Cooperative Extension, published by USDA MyPlate Kitchen, https://www.myplate.gov/recipes/manhattan-clam-chowder

## Scope Statement
This matrix reports allergens identified in the listed ingredients using the stated lookup table version. It covers the US FDA major food allergens and the EU/UK declarable allergens only. It does not assess cross-contact, supplier formulation changes, sulphite concentration, or the preparation environment. It is not an allergen certification. Undetermined cells and unresolved terms require human review before this information is shared with guests.
```

### What this example shows

- **An EU/UK-only allergen is caught.** Clams (L8) produce Molluscs, which is not a US major allergen. A US-only tool reports nothing here.
- **Processed tomato products match their own rows.** Tomato puree (L6) matches the tomato concentrate row and tomato juice (L13) matches the tomato juice row, each cited separately. Neither borrows the fresh-produce row; a word that names a different product form is not a descriptor.
- **This recipe drove a table addition.** In earlier runs on table 1.2, tomato juice matched nothing, the report was PROVISIONAL, and the review queue said `Check supplier label`. The gap was researched against the US standard of identity, approved as row T-086, and the table moved to 1.3 (see `extras/research-mode.md` and the table's change log). Nothing was invented in the meantime — the earlier output said plainly that it could not place the ingredient.
- **A misspelling survives.** L11 reads "2 dashs black peppercorns," exactly as written. No correction, no normalization.
- **"All ingredients" resolves to the whole list.** Step 3 (L18) says "combine all ingredients," which references L3–L14 rather than adding twelve new terms.
- **Every term matched, so no negative is provisional.** Each "Not found" row shows a plain `—`. Compare Example 2, where three unresolved terms put `· open:` on every negative row.
- **Peppercorns are removed before serving, and the matrix does not care.** L20 references the bay leaf and peppercorns; the translator converts what the recipe says and makes no judgment about what ends up on the plate.

---

## Example 4 — Banana Walnut Oatmeal

**Source:** United States Department of Agriculture, USDA's Collection of Nonfat Dry Milk (NDM) Recetas. Found republished by a university extension program rather than on MyPlate Kitchen, so its provenance chain is one step longer than the other three inputs.

### Input

```text
Banana Walnut Oatmeal

Serves 4

Ingredients
2/3 cup milk (non-fat, dry)
1 dash salt
2 3/4 cups water
2 cups oats (quick cooking)
2 bananas (very ripe, mashed)
2 tablespoons maple syrup
2 tablespoons walnuts (chopped)

Directions
1. In a small saucepan, combine reconstituted non-fat dry milk, salt, and additional water. Heat over medium heat until steaming hot, but not boiling.
2. Add oats and cook, stirring until creamy, 1 to 2 minutes.
3. Remove the pan from heat and stir in mashed banana and maple syrup. Divide between 4 bowls, garnish with walnuts, and serve.

Tips and variations
You can substitute 2 cups water or 2 cups skim milk for the reconstituted non-fat dry milk.

Source: United States Department of Agriculture, USDA's Collection of Nonfat Dry Milk (NDM) Recetas
```

### Output

```text
RECIPE: Banana Walnut Oatmeal (L1)
SOURCE: United States Department of Agriculture, USDA's Collection of Nonfat Dry Milk (NDM) Recetas (L17)
TABLE: lookup-table 1.4
STATUS: RESOLVED — all ingredient terms matched.

## At a Glance
Present: Milk (milk (non-fat, dry), skim milk*) · Tree nuts (walnuts) · Other gluten cereals (oats (quick cooking))
Undetermined: —
Note (*): Listed as optional, as an alternative, or as something the dish is served with. May not be used in every preparation — confirm which ingredients were used.
Needs review: —

## Allergen Matrix
| Allergen | Cell | Source |
|---|---|---|
| Milk | Present | L4 → T-002; L16 → T-002 |
| Egg | Not found in listed ingredients | — |
| Fish | Not found in listed ingredients | — |
| Crustacean shellfish | Not found in listed ingredients | — |
| Molluscs | Not found in listed ingredients | — |
| Tree nuts | Present | L10 → T-026 |
| Peanuts | Not found in listed ingredients | — |
| Wheat | Not found in listed ingredients | — |
| Other gluten cereals | Present | L7 → T-045 |
| Soybeans | Not found in listed ingredients | — |
| Sesame | Not found in listed ingredients | — |
| Mustard | Not found in listed ingredients | — |
| Celery | Not found in listed ingredients | — |
| Lupin | Not found in listed ingredients | — |
| Sulphites | Not found in listed ingredients | — |

## Review Queue
None.

## Line Accounting
| ID | Term | Status | Table row | Allergens | Note |
|---|---|---|---|---|---|
| L4 | milk (non-fat, dry) | MATCHED-ALLERGEN | T-002 | Milk | — |
| L5 | salt | MATCHED-NONE | T-070 | — | — |
| L6 | water | MATCHED-NONE | T-072 | — | — |
| L7 | oats (quick cooking) | MATCHED-ALLERGEN | T-045 | Other gluten cereals | — |
| L8 | bananas (very ripe) | MATCHED-NONE | T-077 | — | — |
| L9 | maple syrup | MATCHED-NONE | T-071 | — | — |
| L10 | walnuts | MATCHED-ALLERGEN | T-026 | Tree nuts | — |
| L12 | — | NO-INGREDIENT | — | — | References L4–L6 |
| L13 | — | NO-INGREDIENT | — | — | References L7 |
| L14 | — | NO-INGREDIENT | — | — | References L8–L10 |
| L16 | skim milk | MATCHED-ALLERGEN | T-002 | Milk | Alternative; References L4, L6 |
| L1–L3, L11, L15, L17 | — | NO-INGREDIENT | — | — | No ingredient mentions |

## Numbered Source
L1 Banana Walnut Oatmeal
L2 Serves 4
L3 Ingredients
L4 2/3 cup milk (non-fat, dry)
L5 1 dash salt
L6 2 3/4 cups water
L7 2 cups oats (quick cooking)
L8 2 bananas (very ripe, mashed)
L9 2 tablespoons maple syrup
L10 2 tablespoons walnuts (chopped)
L11 Directions
L12 1. In a small saucepan, combine reconstituted non-fat dry milk, salt, and additional water. Heat over medium heat until steaming hot, but not boiling.
L13 2. Add oats and cook, stirring until creamy, 1 to 2 minutes.
L14 3. Remove the pan from heat and stir in mashed banana and maple syrup. Divide between 4 bowls, garnish with walnuts, and serve.
L15 Tips and variations
L16 You can substitute 2 cups water or 2 cups skim milk for the reconstituted non-fat dry milk.
L17 Source: United States Department of Agriculture, USDA's Collection of Nonfat Dry Milk (NDM) Recetas

## Scope Statement
This matrix reports allergens identified in the listed ingredients using the stated lookup table version. It covers the US FDA major food allergens and the EU/UK declarable allergens only. It does not assess cross-contact, supplier formulation changes, sulphite concentration, or the preparation environment. It is not an allergen certification. Undetermined cells and unresolved terms require human review before this information is shared with guests.
```

### What this example shows

- **Milk and tree nuts, the two most common restaurant allergens.** Non-fat dry milk (L4) matches the milk row and walnuts (L10) the tree nut row. No other test recipe reaches either.
- **A substitution outside the ingredient list is captured.** The tips line (L16) offers skim milk in place of the dry milk. It becomes its own term, marked `Alternative` with an asterisk, and the "2 cups water" in the same line refers back to the listed water (L6) rather than becoming a second water ingredient.
- **An empty review queue reads `None.`** Every term matched a row and no row flags a possible allergen, so there is nothing for a person to check. The section still appears, as the fixed shape requires.
- **Parenthetical detail in an ingredient name survives.** The terms are "milk (non-fat, dry)" and "oats (quick cooking)", exactly as the input wrote them, with only the preparation word "mashed" stripped from the bananas.
- **A publication title is not an ingredient.** The source line (L17) names "Nonfat Dry Milk" in the title of a USDA collection. It produces no term and no reference, and sits in the grouped row.

---

## Example 5 — Glazed Meatloaf, pasted as a raw web page (stress test)

**Source:** the same USDA recipe as Example 1, but copied straight out of a browser with select-all: site buttons, a food-group panel, a related-recipes list, and an 86-line nutrition panel included. 172 lines instead of 35.

**Why it is here:** the judges run inputs nobody prepared for them. This is the same dish as Example 1, so the two outputs can be compared directly. The allergen results are identical.

### Input

`inputs/glazed-meatloaf-page-paste.txt`, reproduced in the Numbered Source below.

### Output

```text
RECIPE: Glazed Meatloaf (L1)
SOURCE: Not in source
TABLE: lookup-table 1.4
STATUS: RESOLVED — all ingredient terms matched.

## At a Glance
Present: Egg (large egg) · Other gluten cereals (rolled oats) · Mustard (yellow mustard)
Undetermined: Soybeans (vegetable oil) · Sulphites (yellow mustard)
Needs review: 3 item(s)
- vegetable oil (L24) — Clarify ingredient
- yellow mustard (L31, L39) — Check supplier label

## Allergen Matrix
| Allergen | Cell | Source |
|---|---|---|
| Milk | Not found in listed ingredients | — |
| Egg | Present | L35 → T-008 |
| Fish | Not found in listed ingredients | — |
| Crustacean shellfish | Not found in listed ingredients | — |
| Molluscs | Not found in listed ingredients | — |
| Tree nuts | Not found in listed ingredients | — |
| Peanuts | Not found in listed ingredients | — |
| Wheat | Not found in listed ingredients | — |
| Other gluten cereals | Present | L36 → T-045 |
| Soybeans | Undetermined | L24 → T-084 (possible) |
| Sesame | Not found in listed ingredients | — |
| Mustard | Present | L31 → T-057; L39 → T-057 |
| Celery | Not found in listed ingredients | — |
| Lupin | Not found in listed ingredients | — |
| Sulphites | Undetermined | L31 → T-057 (possible); L39 → T-057 (possible) |

## Review Queue
1. L24 — "vegetable oil" — Clarify ingredient
2. L31 — "yellow mustard" — Check supplier label
3. L39 — "yellow mustard" — Check supplier label

## Line Accounting
| ID | Term | Status | Table row | Allergens | Note |
|---|---|---|---|---|---|
| L2 | — | NO-INGREDIENT | — | — | References L27–L29, L31, L36–L39 |
| L24 | vegetable oil | MATCHED-POSSIBLE | T-084 | Soybeans (possible) | Oil source not stated; vegetable oil is often soybean oil. |
| L25 | onion | MATCHED-NONE | T-074 | — | — |
| L26 | green bell pepper | MATCHED-NONE | T-077 | — | — |
| L27 | garlic | MATCHED-NONE | T-074 | — | — |
| L28 | dried thyme | MATCHED-NONE | T-075 | — | — |
| L29 | tomato paste | MATCHED-NONE | T-085 | — | — |
| L30 | water | MATCHED-NONE | T-072 | — | — |
| L31 | yellow mustard | MATCHED-ALLERGEN | T-057 | Mustard, Sulphites (possible) | Some prepared mustards contain wine or sulphite preservatives. |
| L32 | salt | MATCHED-NONE | T-070 | — | — |
| L33 | black pepper | MATCHED-NONE | T-070 | — | — |
| L34.1 | ground beef | MATCHED-NONE | T-078 | — | Alternative |
| L34.2 | turkey | MATCHED-NONE | T-078 | — | Alternative |
| L35 | large egg | MATCHED-ALLERGEN | T-008 | Egg | — |
| L36 | rolled oats | MATCHED-ALLERGEN | T-045 | Other gluten cereals | — |
| L38 | tomato paste | MATCHED-NONE | T-085 | — | — |
| L39 | yellow mustard | MATCHED-ALLERGEN | T-057 | Mustard, Sulphites (possible) | Some prepared mustards contain wine or sulphite preservatives. |
| L48 | — | NO-INGREDIENT | — | — | References L24 |
| L50 | — | NO-INGREDIENT | — | — | References L25–L28 |
| L52 | — | NO-INGREDIENT | — | — | References L29, L30, L38 |
| L54 | — | NO-INGREDIENT | — | — | References L25 |
| L58 | — | NO-INGREDIENT | — | — | References L26, L31–L33, L39 |
| L60 | — | NO-INGREDIENT | — | — | References L25, L34.1, L35, L36 |
| L62 | — | NO-INGREDIENT | — | — | References L25 |
| L68 | — | NO-INGREDIENT | — | — | References L29, L31, L37–L39 |
| L70 | — | NO-INGREDIENT | — | — | References L36 |
| L1, L3–L23, L37, L40–L47, L49, L51, L53, L55–L57, L59, L61, L63–L67, L69, L71–L172 | — | NO-INGREDIENT | — | — | No ingredient mentions |

## Numbered Source
L1 Glazed Meatloaf
L2 This family favorite is flavored with garlic, thyme, tomato paste, and mustard then coated with a tangy glaze and a sprinkle of oats.
L3 3.30(106)4 Servings259cal/serving
L4 Save to cookbookAdd to my planner
L5 Log to today
L6 Add to my day
L7 Print in English 🇺🇸
L8 Save to Pinterest
L9 Portions per serving(1 serving)
L10 MyPlate food groups
L11 Fruits
L12 —
L13 Vegetables
L14 1/4 cups
L15 Grains
L16 1/2 ounces
L17 Protein
L18 3 ounces
L19 Dairy
L20 —
L21 What you need
L22 Ingredients
L23 16 items
L24 * 1 teaspoon vegetable oil
L25 * 1 onion, chopped
L26 * 1/2 green bell pepper, cored and diced
L27 * 2 cloves garlic, peeled and diced
L28 * 1 teaspoon dried thyme
L29 * 2 tablespoons tomato paste
L30 * 1/2 cup water
L31 * 1 tablespoon yellow mustard
L32 * 1/2 teaspoon salt
L33 * 1/4 teaspoon black pepper
L34 * 1 pound ground beef or turkey
L35 * 1 large egg
L36 * 1/2 cup rolled oats
L37 * For the Glaze:
L38 * 1 teaspoon tomato paste
L39 * 1 teaspoon yellow mustard
L40 Learn more about
L41 OnionsBell PeppersGarlicHerbs
L42 How it's done
L43 Directions
L44 15 steps👨‍🍳Cook Mode
L45 1. 1
L46 Preheat the oven to 325 °F. Lightly grease a baking sheet.
L47 2. 2
L48 Put a skillet over medium heat and when it is hot, add the oil.
L49 3. 3
L50 Add the onion, bell pepper, garlic, and thyme and cook about 10 minutes until golden.
L51 4. 4
L52 Lower the heat and add the water and tomato paste.
L53 5. 5
L54 Cook about 10 minutes until the onion is soft and most of the liquid has been absorbed.
L55 6. 6
L56 Set aside to cool.
L57 7. 7
L58 Add the mustard, salt and pepper.
L59 8. 8
L60 While the onion mixture is cool, put the ground beef, 4 tablespoons rolled oats and the egg in the bowl and gently mix, by hand.
L61 9. 9
L62 Add the cooled onion mixture and gently mix again until everything is thoroughly incorporated.
L63 10. 10
L64 Do not over mix.
L65 11. 11
L66 Form the mixture into a loaf about 8x4 inches and put on the prepared baking sheet.
L67 12. 12
L68 To make the glaze: Put the tomato paste and mustard in a small bowl and mix well.
L69 13. 13
L70 Rub on top of the meatloaf and sprinkle with the remaining 2 tablespoons rolled oats.
L71 14. 14
L72 Put in the oven to cook about 1 hour until deep brown and cooked throughout.
L73 15. 15
L74 Serve right away or cover and refrigerate up to 2 days.
L75 Keep exploring
L76 * Bread Bake with Chicken, Cheese, and Vegetables
L77 * Pizza Meat Loaf
L78 * Tuna Melt Burger
L79 * Pasta Primavera
L80 Per serving · 1 serving
L81 Nutrition
L82 Calories
L83 259
L84 Protein
L85 23.9g
L86 Carbs
L87 14.1g
L88 Fat
L89 11.74g
L90 Fiber
L91 2.62g
L92 Macronutrients
L93 Total Calories
L94 259.4
L95 Total Fat
L96 11.7 g
L97 Saturated Fat
L98 4 g
L99 Monounsaturated Fat
L100 5 g
L101 Polyunsaturated Fat
L102 1 g
L103 Linoleic Acid
L104 1 g
L105 α-Linolenic Acid
L106 0.1 g
L107 Omega 3 - EPA
L108 0 g
L109 Omega 3 - DHA
L110 0 g
L111 Cholesterol
L112 117.1 mg
L113 Carbohydrates
L114 14.1 g
L115 Dietary Fiber
L116 2.6 g
L117 Total Sugars
L118 3.4 g
L119 Added Sugars included
L120 0 g
L121 Protein
L122 23.9 g
L123 Minerals
L124 Calcium
L125 50 mg
L126 Potassium
L127 528 mg
L128 Sodium
L129 434.5 mg
L130 Copper
L131 0 mg
L132 Iron
L133 4 mg
L134 Magnesium
L135 46 mg
L136 Phosphorus
L137 263 mg
L138 Selenium
L139 26 mg
L140 Zinc
L141 6 mg
L142 Vitamins
L143 Vitamin A
L144 36 mcg
L145 Vitamin B6
L146 0.4 mg
L147 Vitamin B12
L148 2.3 mcg
L149 Vitamin C
L150 15 mg
L151 Vitamin D
L152 0 mcg
L153 Vitamin E
L154 1 mg
L155 Vitamin K
L156 11 mcg
L157 Folate
L158 29 mcg
L159 Thiamin
L160 0.1 mg
L161 Riboflavin
L162 0.2 mg
L163 Niacin
L164 5 mg
L165 Choline
L166 118 mg
L167 More main dish recipes
L168 Browse all 1,072
L169 Bread Bake with Chicken, Cheese, and Vegetables
L170 3.9·259 cal
L171 Pizza Meat Loaf
L172 3.7·258 cal

## Scope Statement
This matrix reports allergens identified in the listed ingredients using the stated lookup table version. It covers the US FDA major food allergens and the EU/UK declarable allergens only. It does not assess cross-contact, supplier formulation changes, sulphite concentration, or the preparation environment. It is not an allergen certification. Undetermined cells and unresolved terms require human review before this information is shared with guests.
```

### What this example shows

- **Related-recipe titles do not leak into the matrix.** The "Keep exploring" list names "Bread Bake with Chicken, **Cheese**, and Vegetables" (L76), "**Tuna** Melt Burger" (L78), and "**Pasta** Primavera" (L79), and the footer repeats two of them (L169, L171). Milk, Fish, and Wheat all come back **Not found**. A keyword matcher reports three allergens the dish does not contain.
- **The food-group and nutrition panels produce nothing.** "Dairy" (L19), "Protein" (L17), "Calcium" (L124), and the other 80-odd nutrition lines are inside non-recipe sections and yield no terms.
- **Site links produce nothing.** "Learn more about / OnionsBell PeppersGarlicHerbs" (L40–L41) is skipped, where an earlier version of the rules turned "Herbs" into an unresolved term.
- **The title is found, not guessed.** "Glazed Meatloaf" (L1) wins over the site's buttons ("Save to Pinterest," "Print in English") and over the recipe's own description sentence (L2), because page controls and prose are excluded from title candidates.
- **SOURCE is honest.** The page paste carries no attribution line, so the field reads `Not in source` rather than borrowing the URL from anywhere else.
- **All 172 lines are accounted for once.** 26 own rows plus one grouped row covering 146 lines. Numbered Source reproduces every line verbatim, including the emoji at L7 and L44 and the Greek character at L105.
- **Identical results to the clean input.** Example 1 and this run report the same three Present allergens, the same two Undetermined, and the same three review items, with only the line numbers differing. The clutter changed nothing but the citations.
