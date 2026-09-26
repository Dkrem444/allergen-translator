# Open Items — Questions, Tests, and Optimizations

> **Not part of the translator. Do not add this file to the Claude project knowledge.** It describes table rows that may not exist yet; loading it into a run could put an unapproved mapping into an output.

Working notes. Kept so nothing gets lost between now and after the deadline.

## Resolved

- [x] **Milk and tree nuts tested.** Banana Walnut Oatmeal added as Example 4; both rows now exercised. Ten of the fifteen rows have been reported as Present or Undetermined.
- [x] **Tomato juice** → T-086, assigns nothing, per 21 CFR 156.145 (salt and organic acid only).
- [x] **Canned and stewed tomatoes** → T-087 and T-088, flagging celery, per 21 CFR 155.190. The standard permits celery up to 10% by weight and requires no declaration of onion, peppers, or celery for the stewed style.
- [x] **Composite items** → T-089 croutons and T-090 sausage flag the allergens a supplier label should be checked for and assign nothing, reason "Composition varies by supplier."
- [x] **Vinegars (plain, rice, cider):** deliberately excluded. Sulphite content depends on the producer and no authority covers the category, so they stay in the review queue.
- [x] **"Top with" / "sprinkle with" / "serve with":** decided these do **not** mark a term optional. Only explicit language does (optional, as desired, if desired, if using, to garnish). A topping the recipe instructs you to add is an ingredient.
- [x] **`examples.md` in the project:** yes, added last. It ships in the repo, so a judge's project will have it; it stays out while rules or table changes are being tested.
- [x] **Repo-only files marked.** `extras/research-mode.md` and this file now carry a do-not-load warning at the top.

## Blocking before submission

- [ ] Confirm the repo file tree in README.md matches what actually gets committed.

## Tests run and passed

- [x] **House-made sub-recipe** ("our house chimichurri") → `Check house recipe` fired; all negatives provisional.
- [x] **Free-from claims** ("gluten-free panko," "vegan butter") → both `UNRESOLVED`, neither matched the unqualified row.
- [x] **Brand names** ("Lea & Perrins Worcestershire," "Grey Poupon Dijon," "Old Bay") → generic rows matched with brands kept in the term; a brand with no generic name stayed unresolved; `Confirm brand` fired.
- [x] **Non-recipe input** (meeting notes) → `STATUS: NO INGREDIENTS FOUND`, full shape held, nothing invented.
- [x] **Raw 172-line web page paste** → allergen results identical to the clean file; related-recipe titles naming Cheese, Tuna, and Pasta produced no Milk, Fish, or Wheat (Example 5).
- [x] **Reproducibility** → nine Meatloaf runs across four table versions; every allergen result identical.
- [x] **Footnote marker bug** → a leading `* ` was being rendered as a markdown bullet and dropped. Rewritten as `Note (*):`; confirmed working.
- [x] **Accompaniments** → "pour over steak" now marked `Serving` rather than counted as an ingredient of the dish.

## Tests not yet run

- [ ] **A very short input** (a title and two ingredients), to confirm every section still appears.
- [ ] **A fish or shellfish stock** ("fish broth"), to confirm the stock split actually surfaces Fish. This is the reason the split rule exists and it has only been tested with chicken.
- [ ] **A recipe with a garbled character** — an encoding artifact ("400 Â°F") is reproduced verbatim, which is correct per the rules but leaves the garble in the output. Decide whether that is worth an exception.

## Open questions

- **Bacon, deli meats, and other cured products.** Still unresolved by design. Sausage and croutons now have flagging rows; these could follow the same pattern if they come up often.
- **Ketchup, tomato sauce, and salsa.** Excluded from the tomato rows because their formulations vary. Candidates for flagging rows in the T-089 style.
- **Vegetable juice blends.** Explicitly excluded from T-086 because they commonly contain celery. A flagging row would beat leaving them unresolved.
- **Category rows** ("any named cheese," "any named fish") are the one place the translator uses general knowledge to identify what an ingredient is. Acceptable, but worth naming in feedback if a judge raises it.
- **Sub-recipe carry-up.** A recipe that references a house sauce cannot inherit that sauce's allergens today. This is the biggest functional gap for real restaurant use, and a post-deadline feature rather than a rule tweak.

## After the deadline

- **Multi-recipe menu matrix.** One table across a whole menu, dishes as rows and allergens as columns. This is what a restaurant actually posts, and it is a second tool, not a rule change.
- **CSV export** shaped to fit existing allergen tools.
- **A "reviewed by" sign-off field,** matching the human-approval step commercial tools use.
- **Point the same pattern at another conversion.** The engine here — cite every field, never guess, send gaps to a review queue, grow a versioned table — fits invoice-to-inventory and sales-to-per-server reporting, which are closer to the consulting work.
