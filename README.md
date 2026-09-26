# Recipe → Allergen Matrix Translator

Drop this folder into a Claude project, give it a recipe, and get back a fixed-format allergen report where every result cites the line of the recipe and the table row it came from, and every gap is marked instead of guessed.

Built for Weekly Comp #13: The Translator.

## What it converts

| From | To |
|---|---|
| One recipe as text — title, ingredient list, and optionally directions, notes, and a source line | A seven-section allergen report covering the US FDA major food allergens (9) and the EU/UK declarable allergens (14), as 15 fixed rows |

Somebody does this by hand today. A restaurant onboarding an allergen matrix has a binder or a shared doc full of recipes, and someone reads each one, decides which allergens are in it, and types the answer into a spreadsheet or a compliance tool. This translator does the reading and the tracing; a person reviews the short list of flagged items.

## Setup

1. Create a Claude project.
2. Add these files to the project's knowledge: `identity.md`, `rules.md`, `reference/output-schema.md`, `reference/lookup-table.md`, and `examples.md`.
3. Set the project's custom instructions to:

```
You are a recipe-to-allergen translator. When the user provides a recipe, follow rules.md exactly and return output in the format defined in output-schema.md. Use lookup-table.md as the only source for connecting ingredients to allergens. Do not search the web or use outside knowledge to assign allergens. Return only the translator output, with no introduction, commentary, or summary.
```

## Use

Start a new chat in the project, paste or attach one recipe, and send:

```
Translate this recipe.
```

Start a fresh chat for each recipe. Each run is meant to be independent.

**What to feed it:** the recipe title, the ingredient list, and the directions. Notes and a source line are welcome. Pasting a whole web page works: site buttons, food-group and nutrition panels, comment sections, and related-recipe lists are skipped for matching, and the title is picked from the page's own headings rather than its buttons. Example 5 in `examples.md` is a 172-line raw browser paste whose allergen results are identical to the clean 35-line file.

**Why the output is long on a messy input.** Numbered Source reproduces every input line verbatim, on purpose: it is what makes each citation checkable, and it is the proof that no line was quietly discarded. Line Accounting is where the clutter collapses — on that 172-line paste it is 26 rows plus one grouped row. Read At a Glance and the review queue at the top; the sections below exist for auditing.

**What comes back:**

```
RECIPE: Glazed Meatloaf (L1)
SOURCE: USDA Center for Nutrition Policy and Promotion, MyPlate Kitchen, ... (L35)
TABLE: lookup-table 1.3
STATUS: RESOLVED — all ingredient terms matched.

## At a Glance
Present: Egg (large egg) · Other gluten cereals (rolled oats) · Mustard (yellow mustard)
Undetermined: Soybeans (vegetable oil) · Sulphites (yellow mustard)
Needs review: 3 item(s)
- vegetable oil (L3) — Clarify ingredient
- yellow mustard (L10, L18) — Check supplier label
```

Below that: the 15-row matrix with a citation in every filled cell, the full review queue, line-by-line accounting for every input line, the numbered source, and a fixed scope statement. Full examples are in `examples.md`.

## How to read the output

**Three cell values, and only three:**

| Value | Meaning |
|---|---|
| **Present** | An ingredient matched a table row that assigns this allergen. |
| **Undetermined** | An ingredient may contain it, depending on brand, recipe, or processing. |
| **Not found in listed ingredients** | Nothing in the listed ingredients indicated it. **This is not a claim that the dish is free of it.** |

**Two status values to watch:**

- `RESOLVED` — every ingredient matched a table row.
- `PROVISIONAL` — at least one ingredient is not in the table. Every "Not found" row then carries `· open: L13`, naming the unresolved lines, because an unidentified ingredient could contain anything.

**The review queue is the point.** It is the short list of things a person needs to check: a supplier label, a brand, a house recipe, or a vague ingredient to clarify. On a 35-line recipe it was 3 items.

## Design decisions a reader should know

**The lookup table is a cited source, not invention.** The input says "Parmesan"; the output says Milk. The word "milk" is not in the recipe. It comes from row T-004 of `reference/lookup-table.md`, and the output cites both the line and the row (`L8 → T-004`), so a reader can open the table and check the mapping. This is how commercial allergen software works too: allergens are recorded at the ingredient level and propagate to dishes. The difference here is that every propagation is traceable to a written row, and the table is versioned, so an output produced under 1.3 can be checked against 1.3.

**"Assigns" versus "Possible" is a strict line.** A row assigns an allergen only when the ingredient is made from it by definition — butter is milk. When composition varies by brand, the row flags it as possible, which produces Undetermined instead of Present. Soy sauce assigns soy and only flags wheat, even though most soy sauce is brewed with wheat.

**Every alternative is reported.** When a recipe says "spaghetti noodles (or thin flat egg noodles)," both become terms, so Egg is reported even though a cook using plain spaghetti would have none. Both terms are marked `Alternative` and carry an asterisk in At a Glance with a footnote telling the reader to confirm which was used. Reporting an allergen that might not be in the dish is the safer error.

**A missing ingredient is never filled in.** Hot pepper sauce is not in the table, so it comes back `UNRESOLVED` with `Check supplier label`, and the whole report turns PROVISIONAL. The table grows only through reviewed, cited additions (see `extras/research-mode.md`), never during a run.

**The translator never searches the web.** Every allergen result comes from the recipe and the table. That is what makes the output reproducible: the same recipe under the same table version returns the same report.

## Compared to keyword-matching tools

Free allergen checkers exist that take a pasted ingredient list and return the allergen groups they find. Running `inputs/glazed-meatloaf.txt` through one of them returned six allergen groups, including celery and sesame. Neither word appears anywhere in that recipe. The tool's own "found in" evidence pointed at fragments of unrelated directions ("and thyme and cook about 10 minutes until golden"), and both allergens were carried into a suggested "Contains:" label statement.

That is the failure this translator is built against, and the differences are structural, not a matter of tuning:

| | Keyword tool | This translator |
|---|---|---|
| Evidence | A quoted fragment, with no way to see why it matched | A line number and a table row: `L15 → T-045` |
| Verdicts | One: found | Three: Present, Undetermined, Not found in listed ingredients |
| Unknown ingredient | Silently absent from the results | `UNRESOLVED`, report turns PROVISIONAL, ingredient goes to the review queue |
| Output | Guest-facing "Contains:" wording | A reviewed draft; the scope statement rules out certification |
| Reproducibility | No version stated | Output names the lookup table version it was produced under |

The same recipe through this translator returns Egg, Other gluten cereals, and Mustard as Present, Soybeans and Sulphites as Undetermined, and three items for review. Every one of those results cites a line of the recipe and a row of the table, and the two allergens that tool invented are simply absent, because nothing in the input supports them.

The comparison is not that keyword tools are useless. They are fast, free, and need no setup, which matters. It is that a fast wrong answer and a slow right one are not interchangeable when the output is going to be used to answer a guest's question about what they can eat.

## What it does not do

- Cross-contact, shared equipment, and preparation environment: not assessed.
- Sulphite concentration: recipes never state it, so the sulphite row reflects ingredient type only.
- Supplier labels and brand formulations: not read. That is what the review queue is for.
- Allergens outside the FDA 9 and EU/UK 14 (corn, coconut): recorded as out-of-scope notes, never placed in the matrix.
- Spelling: never corrected. "2 dashs black peppercorns" is reproduced exactly.

This produces a reviewed draft. It is not an allergen certification, and what a restaurant tells a guest stays a human decision.

## Files

```
identity.md          what it converts, from what, to what
rules.md             the procedure: line numbering, term extraction, matching, cells, review queue
examples.md          five worked examples with their outputs and what each one demonstrates
verification.md      one output traced claim by claim back to the input
reference/
  output-schema.md   the contract: allergen set, cell values, statuses, every section's format
  lookup-table.md    90 ingredient rows (T-001 to T-090), versioned, with a change log
inputs/
  glazed-meatloaf.txt        USDA CNPP — the input traced in verification.md
  veggie-chow-mein.txt       University of Illinois Extension, via USDA MyPlate Kitchen
  manhattan-clam-chowder.txt Cornell Cooperative Extension, via USDA MyPlate Kitchen
  banana-walnut-oatmeal.txt  USDA Collection of Nonfat Dry Milk Recetas
  glazed-meatloaf-page-paste.txt  the same recipe copied raw from a browser, 172 lines
extras/
  research-mode.md   how the lookup table grows: gaps → cited proposals → human approval
notes/
  open-items.md      working notes: untested paths, open questions, later ideas
```

## Inputs in this repo

The four recipes in `inputs/` are real and public. `glazed-meatloaf.txt` was written by the USDA's Center for Nutrition Policy and Promotion, a federal work in the public domain. The other two were contributed to USDA MyPlate Kitchen by the University of Illinois Extension Service and Cornell Cooperative Extension, and each file names its source. MyPlate.gov was retired in January 2026; the recipes are preserved at myplate.food, which links each one to its original myplate.gov address.

`banana-walnut-oatmeal.txt` is credited to USDA's Collection of Nonfat Dry Milk (NDM) Recetas and is Example 4; it exercises the Milk and Tree nuts rows that no other input reaches.

Every output in `examples.md` was produced by running these files through the translator in a fresh chat on lookup table 1.3. The Numbered Source in Example 1 matches `inputs/glazed-meatloaf.txt` line for line, all 35 lines.
