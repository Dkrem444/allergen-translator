# Research Mode — How the Lookup Table Grows

> **Not part of the translator. Do not add this file to the Claude project knowledge.** It contains instructions for searching the web, which the translator must never do during a run. The project takes five files only, listed in `README.md`.


**This is not part of the translator.** It is a separate step, run in a separate chat, that turns the review queue's unresolved ingredients into proposed table rows for a person to approve or reject.

The translator itself never searches the web and never adds a row. That is deliberate: every allergen result must trace to the input or to a table row that already existed when the run happened, which is what makes a run reproducible and checkable. Research mode sits outside that boundary so it cannot contaminate an output.

## The loop

```
1. Run a recipe through the translator
     → matrix + review queue, built only from the recipe and the table

2. Take the UNRESOLVED terms from the review queue into research mode
     → proposed rows, each with a source, marked PROPOSED — not applied

3. A person approves or rejects each proposal
     → approved rows are added to lookup-table.md, version incremented,
       change log updated

4. Re-run the recipe
     → the previously unresolved term now matches, and the status
       may move from PROVISIONAL to RESOLVED
```

Step 3 is the gate. Nothing from a search reaches a matrix without a person agreeing to it first.

## Running it

In a **new chat**, not one where a translation was run:

```
Research mode. Here are unresolved terms from a translator run:
- tomato juice
- hot pepper sauce
- croutons

For each term, follow extras/research-mode.md.
```

## Rules for a research pass

1. **Propose, never apply.** Output is a list of proposals. Every proposal is marked `PROPOSED — not applied`. Research mode does not rewrite `lookup-table.md`.
2. **Every proposal cites a source.** Give the specific source (a regulator's guidance, a standard of identity, a manufacturer's ingredient statement) with a URL and the date retrieved. A proposal with no source is not written.
3. **Assign only what a definition or standard supports.** If a regulation or standard of identity says the product is made from the allergen, propose it under Assigns. If the allergen is common but formulation varies, propose it under Possible, with the variability as the reason. When in doubt, Possible.
4. **Composite and supplier-specific items get no row.** Croutons, sausage, bacon, deli meat, house sauces, and branded prepared foods vary by supplier. The honest answer lives on a label, not in a table. For these, return `CHECK LABEL — no row proposed` and say which allergens to look for on the label.
5. **A term too vague to identify gets no row.** "Oil," "spices," and "other vegetables" cannot be resolved by research. Return `CLARIFY — no row proposed`.
6. **Stay inside the allergen set.** Only the 15 allergens in `reference/output-schema.md`. An out-of-scope allergen (corn, coconut) may be proposed as a row that assigns nothing, with an `Out of scope:` note.
7. **Do not propose a row that already exists.** Check the table first. If a term should match an existing row, propose adding the term's wording to that row's Matches column instead of creating a new row.

## Proposal format

````text
PROPOSAL 1 — tomato juice
Status: PROPOSED — not applied
Proposed row: new, next unused ID
Matches: tomato juice, canned tomato juice
Assigns: None
Possible: —
Reason / note: <what the source establishes>
Source: <publication or regulation, URL, retrieved YYYY-MM-DD>
Confidence: <high / medium / low, and why>

PROPOSAL 2 — croutons
Status: CHECK LABEL — no row proposed
Why: composition varies by supplier; wheat is near-universal but milk, egg,
     soy, and sesame depend on the product.
Look for on the label: Wheat, Milk, Egg, Soybeans, Sesame
````

## Approving a proposal

1. Read the source yourself. A proposal is a lead, not a finding.
2. Add the row to `reference/lookup-table.md` under its allergen group, using the next unused ID. Never reuse or renumber an ID, so citations in earlier outputs stay valid.
3. Increment the version at the top of the table and add a line to the change log saying what was added and on what basis.
4. Re-run any recipe that depended on the gap. Outputs cite the table version, so an old output stays checkable against the version it was produced under.

## Worked example — how T-085 was added

Tomato paste was UNRESOLVED in the first two runs of `inputs/glazed-meatloaf.txt`, which put the whole report at PROVISIONAL over an ingredient that plainly should be identifiable.

1. **Gap:** "tomato paste" matched no row. The fresh-produce row could not be stretched to cover it, because a word that names a different product form is not a descriptor (`rules.md`, section 6).
2. **Research:** the US standard of identity for tomato concentrates (21 CFR 155.191) defines tomato paste and puree as concentrated tomato liquid, with optional lemon juice, concentrated lemon juice, or organic acids.
3. **Proposal:** a row matching tomato paste, puree, pulp, and concentrate, assigning nothing, citing the standard, and excluding tomato sauce, ketchup, salsa, and canned tomatoes, whose standards allow added flavorings.
4. **Approved and applied** as T-085. Table version went 1.0 → 1.1 with a change log entry.
5. **Re-run:** tomato paste matched, the status moved from PROVISIONAL to RESOLVED, and the review queue dropped from 5 items to 3.

Tomato juice, canned tomatoes, and stewed tomatoes were left out of that row and researched separately in the next pass, which produced table 1.3:

- **T-086 tomato juice** assigns nothing. Its standard of identity (21 CFR 156.145) allows only salt and organic acid.
- **T-087 canned tomatoes** and **T-088 stewed tomatoes** flag **celery** as possible. The canned tomato standard (21 CFR 155.190) permits onion, peppers, and celery up to 10 percent by weight plus spices and flavorings, and the labeling rule requires no declaration of onion, peppers, or celery for the stewed style. A plain-looking canned ingredient can carry a regulated allergen without saying so.
- **T-089 croutons** and **T-090 sausage** assign nothing and flag the allergens a supplier label should be checked for, with the reason "Composition varies by supplier." A composite item gets a row that says what to look for, never a row that decides.

Plain, rice, and cider vinegars were researched and deliberately left out: sulphite content depends on the producer and no authority covers the category, so they keep landing in the review queue. A gap the table admits to is worth more than a row that overclaims.
