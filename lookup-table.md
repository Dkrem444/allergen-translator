# Lookup Table — Ingredient → Allergen

**Version:** 1.4
**Rows:** T-001 to T-090
**Allergen set:** as defined in `output-schema.md`

This table is the only source the translator uses to connect an ingredient to an allergen. The procedure for matching an input line to a row is defined in `rules.md`.

---

## Row Principles

1. **Assigns** lists an allergen only when the ingredient is made from that allergen by definition (butter is made from milk). An assigned allergen produces a **Present** cell. Oils made from an allergen assign it even though highly refined versions may be exempt from labeling, because a recipe line does not state how an oil was refined. The exemption is recorded in the note.
2. **Possible** lists an allergen when the ingredient commonly contains it but its composition varies by brand, recipe, or processing. A possible allergen produces an **Undetermined** cell unless another term assigns it.
3. **Reason / note** states why an allergen is possible, or records an out-of-scope allergen. Out-of-scope notes are prefixed `Out of scope:`.
4. `None` in Assigns means the row assigns no allergen in the set. It does not mean the ingredient is allergen-free.
5. An ingredient absent from this table is not a claim of any kind. A term that matches no row is `UNRESOLVED`.
6. Versions of an ingredient qualified by a free-from or substitute claim ("vegan," "plant-based," "gluten-free," "dairy-free," "egg-free," "nut-free," "soy-free") do not match the unqualified row. They are `UNRESOLVED` unless a row names the qualified version.
7. Processed or prepared items (sausage, bacon, deli meats) do not match raw-ingredient rows. Brand names are handled in `rules.md`.

---

## Table

### Milk

| ID | Matches | Assigns | Possible | Reason / note |
|---|---|---|---|---|
| T-001 | butter, salted butter, unsalted butter | Milk | — | — |
| T-002 | milk, whole milk, skim milk, 2% milk, buttermilk, evaporated milk, condensed milk, sweetened condensed milk, milk powder, goat milk, sheep milk | Milk | — | The FDA counts milk from cows, goats, sheep, and other domesticated ruminants. |
| T-003 | cream, heavy cream, whipping cream, half-and-half, sour cream, crème fraîche | Milk | — | — |
| T-004 | cheese, and any named cheese (e.g., parmesan, cheddar, mozzarella, feta, ricotta, gruyère, pecorino, cream cheese, goat cheese) | Milk | — | Includes cheese made from goat, sheep, or buffalo milk. |
| T-005 | yogurt, Greek yogurt | Milk | — | — |
| T-006 | ghee, clarified butter | Milk | — | — |
| T-007 | whey, casein, lactose | Milk | — | — |

### Egg

| ID | Matches | Assigns | Possible | Reason / note |
|---|---|---|---|---|
| T-008 | egg, eggs, egg yolk, egg yolks, egg white, egg whites, whole egg, duck egg, quail egg | Egg | — | The FDA counts eggs from domesticated chickens, ducks, geese, quail, and other fowl. |
| T-009 | mayonnaise, mayo | Egg | Mustard | Mustard is a common but not universal mayonnaise ingredient. |
| T-010 | aioli | — | Egg, Mustard | Traditional aioli is garlic and oil; many versions add egg yolk and mustard. |
| T-011 | egg noodles | Egg, Wheat | — | — |

### Fish

| ID | Matches | Assigns | Possible | Reason / note |
|---|---|---|---|---|
| T-012 | anchovy, anchovies, anchovy fillets, anchovy paste | Fish | — | — |
| T-013 | fish, and any named fish (e.g., salmon, tuna, cod, halibut, trout, tilapia, sardines, mackerel, bass, snapper) | Fish | — | — |
| T-014 | fish sauce | Fish | — | — |
| T-015 | bonito flakes, katsuobushi | Fish | — | — |
| T-016 | dashi | — | Fish | Most dashi uses bonito; some versions use only kombu. |
| T-017 | Worcestershire sauce | — | Fish, Other gluten cereals | Many brands contain anchovy; some use barley malt vinegar. Formulation is brand-dependent. |

### Crustacean shellfish

| ID | Matches | Assigns | Possible | Reason / note |
|---|---|---|---|---|
| T-018 | shrimp, prawns | Crustacean shellfish | — | — |
| T-019 | crab, lobster, crayfish, langoustine | Crustacean shellfish | — | — |
| T-020 | shrimp paste | Crustacean shellfish | — | — |
| T-021 | curry paste | — | Crustacean shellfish, Fish | Many Thai curry pastes contain shrimp paste; some contain fish sauce. Formulation is brand-dependent. |

### Molluscs

| ID | Matches | Assigns | Possible | Reason / note |
|---|---|---|---|---|
| T-022 | clams, mussels, oysters, scallops | Molluscs | — | — |
| T-023 | squid, calamari, octopus, snails, escargot | Molluscs | — | — |
| T-024 | oyster sauce | Molluscs | Wheat, Soybeans | Many brands add wheat flour or soy. |

### Tree nuts

| ID | Matches | Assigns | Possible | Reason / note |
|---|---|---|---|---|
| T-025 | almonds, almond flour, almond meal, almond milk, almond butter | Tree nuts | — | — |
| T-026 | walnuts, pecans, cashews, pistachios, hazelnuts, macadamia nuts, Brazil nuts | Tree nuts | — | — |
| T-027 | pine nuts, pinon nuts | Tree nuts | — | On the US FDA tree nut list; not on the EU/UK tree nut list. |
| T-028 | marzipan, almond paste | Tree nuts | Egg | Some marzipan contains egg white. |
| T-029 | pesto | — | Tree nuts, Milk | Traditional pesto contains pine nuts and cheese; recipes vary. |
| T-030 | mixed nuts, nuts (type not stated), nut butter (type not stated), praline | — | Tree nuts, Peanuts | Nut type not stated. |

### Peanuts

| ID | Matches | Assigns | Possible | Reason / note |
|---|---|---|---|---|
| T-031 | peanuts, peanut butter, peanut flour, peanut sauce | Peanuts | — | — |
| T-032 | peanut oil, groundnut oil | Peanuts | — | The US exempts highly refined peanut oil from allergen labeling; the EU/UK does not. |
| T-033 | satay sauce | — | Peanuts, Soybeans | Most satay sauce is peanut-based; many contain soy sauce. |

### Wheat

| ID | Matches | Assigns | Possible | Reason / note |
|---|---|---|---|---|
| T-034 | all-purpose flour, bread flour, cake flour, pastry flour, whole wheat flour, self-rising flour, wheat flour | Wheat | — | — |
| T-035 | flour (type not stated) | — | Wheat | Flour type not stated. |
| T-036 | semolina, durum, spelt, kamut, khorasan wheat, farro, bulgur, couscous | Wheat | — | — |
| T-037 | bread, baguette, pita, flour tortilla, pizza dough | Wheat | Milk, Egg, Soybeans, Sesame | Bread formulations vary. |
| T-038 | brioche | Wheat, Egg, Milk | — | — |
| T-039 | breadcrumbs, panko | Wheat | Milk, Egg, Soybeans, Sesame | Commercial breadcrumb formulations vary. |
| T-040 | pasta, spaghetti, penne, linguine, fettuccine, macaroni, lasagna noodles | Wheat | Egg | Some pasta contains egg. |
| T-041 | seitan, vital wheat gluten | Wheat | — | — |
| T-042 | batter, breading, coating mix | — | Wheat, Egg, Milk | Composition not stated. |
| T-083 | glucose syrup, dextrose, maltodextrin | — | Wheat | Source grain not stated; may be wheat. The EU/UK exempts wheat-based glucose syrups and maltodextrins from declaration; the US has no equivalent blanket exemption. |

### Other gluten cereals

| ID | Matches | Assigns | Possible | Reason / note |
|---|---|---|---|---|
| T-043 | barley, pearl barley, barley malt, malt extract, malt vinegar | Other gluten cereals | — | — |
| T-044 | rye, rye flour, rye bread, pumpernickel | Other gluten cereals | Wheat | Rye breads commonly include wheat flour. |
| T-045 | oats, rolled oats, oat flour, oat milk | Other gluten cereals | — | The EU/UK list includes oats among gluten cereals. |
| T-046 | beer, ale, lager, stout | Other gluten cereals | Wheat | Beer is brewed from barley; some beers also use wheat. |

### Soybeans

| ID | Matches | Assigns | Possible | Reason / note |
|---|---|---|---|---|
| T-047 | soy sauce, shoyu | Soybeans | Wheat | Most soy sauces are brewed with wheat; some are not. |
| T-048 | tamari | Soybeans | Wheat | Tamari is often made without wheat, but not always. |
| T-049 | tofu, tempeh, edamame, soybeans, soy milk | Soybeans | — | — |
| T-050 | miso | Soybeans | Other gluten cereals | Some miso is made with barley. |
| T-051 | soy lecithin | Soybeans | — | — |
| T-052 | soybean oil, soy oil | Soybeans | — | Highly refined soybean oil is exempt from US and EU/UK allergen labeling. |
| T-084 | vegetable oil | — | Soybeans | Oil source not stated; vegetable oil is often soybean oil. |

### Sesame

| ID | Matches | Assigns | Possible | Reason / note |
|---|---|---|---|---|
| T-053 | sesame seeds, sesame oil, toasted sesame oil, tahini | Sesame | — | The EU/UK has no exemption for sesame oil. |
| T-054 | hummus | — | Sesame | Traditional hummus contains tahini; recipes vary. |
| T-055 | za'atar, everything bagel seasoning | — | Sesame | These blends commonly contain sesame seeds. |
| T-056 | furikake | — | Sesame, Fish, Soybeans | Blends commonly contain sesame, bonito, or soy; formulations vary. |

### Mustard

| ID | Matches | Assigns | Possible | Reason / note |
|---|---|---|---|---|
| T-057 | mustard, yellow mustard, Dijon mustard, whole grain mustard, mustard seed, mustard powder | Mustard | Sulphites | Some prepared mustards contain wine or sulphite preservatives. |
| T-058 | curry powder | — | Mustard, Celery | Curry blends commonly include mustard seed or celery seed. |

### Celery

| ID | Matches | Assigns | Possible | Reason / note |
|---|---|---|---|---|
| T-059 | celery, celery stalks, celery leaves, celeriac, celery root, celery seed, celery salt | Celery | — | — |
| T-060 | mirepoix | Celery | — | Mirepoix is onion, carrot, and celery by definition. |
| T-061 | stock, broth, bouillon, stock cube (any type — see `rules.md` for source modifiers) | — | Celery, Wheat, Soybeans | Stocks commonly include celery; bouillon products may contain wheat or soy. |
| T-087 | canned tomatoes, diced tomatoes, whole peeled tomatoes, crushed tomatoes | — | Celery | US standard of identity (21 CFR 155.190) permits onion, peppers, and celery up to 10% by weight, plus salt, spices, and flavorings. Contents vary by brand. |
| T-088 | stewed tomatoes | — | Celery | 21 CFR 155.190 permits celery in stewed tomatoes and requires no declaration of onion, peppers, or celery for that style. Contents vary by brand. |

### Lupin

| ID | Matches | Assigns | Possible | Reason / note |
|---|---|---|---|---|
| T-062 | lupin, lupine, lupin flour, lupini beans | Lupin | — | — |

### Sulphites

| ID | Matches | Assigns | Possible | Reason / note |
|---|---|---|---|---|
| T-063 | sodium metabisulphite, potassium metabisulphite, sodium sulphite, sulphur dioxide (and "-sulfite" spellings) | Sulphites | — | — |
| T-064 | wine, red wine, white wine, sherry, port, vermouth | — | Sulphites | Sulphites are common in wine; concentration is not stated. |
| T-065 | red wine vinegar, white wine vinegar, balsamic vinegar, sherry vinegar | — | Sulphites | Wine-based vinegars commonly contain sulphites; concentration is not stated. |
| T-066 | dried apricots, raisins, golden raisins, dried figs, prunes, dried cranberries, dried fruit | — | Sulphites | Dried fruit is often treated with sulphites. |
| T-067 | lemon juice, lime juice, bottled lemon juice, lemon juice from concentrate | — | Sulphites | Some bottled juices contain sulphite preservatives (e.g., sodium metabisulfite); others do not. Brand-dependent. |

### Mixed or unspecified

| ID | Matches | Assigns | Possible | Reason / note |
|---|---|---|---|---|
| T-068 | spices, spice blend, seasoning, seasoning mix | — | Celery, Mustard, Sesame | Blend contents not stated. |
| T-069 | chocolate, chocolate chips | — | Milk, Soybeans | Chocolate often contains milk solids or soy lecithin. |
| T-089 | croutons | — | Wheat, Milk, Egg, Soybeans, Sesame | Composition varies by supplier. |
| T-090 | sausage, breakfast sausage, Italian sausage, sausage meat | — | Wheat, Other gluten cereals, Milk, Soybeans, Mustard, Celery, Sulphites | Composition varies by supplier. |

### No allergen in the set

| ID | Matches | Assigns | Possible | Reason / note |
|---|---|---|---|---|
| T-070 | salt, kosher salt, sea salt, black pepper, white pepper, black peppercorns, white peppercorns, peppercorns | None | — | Whole peppercorns match this row, not the single-spice row (T-076). |
| T-071 | sugar, brown sugar, powdered sugar, honey, maple syrup | None | — | — |
| T-072 | water, ice | None | — | — |
| T-073 | olive oil, extra virgin olive oil, canola oil, avocado oil, grapeseed oil | None | — | — |
| T-074 | garlic, onion, shallot, scallion, green onion, leek | None | — | — |
| T-075 | any single named herb, fresh or dried (e.g., parsley, basil, cilantro, thyme, rosemary, oregano, dill, mint, sage, chives, bay leaf) | None | — | — |
| T-076 | any single named ground or whole spice (e.g., cumin, paprika, cinnamon, nutmeg, turmeric, cayenne, ginger, cloves, cardamom, coriander) | None | — | Blends do not match this row. |
| T-077 | fresh lemon juice, fresh lime juice, juice of a lemon, juice of a lime, lemon, lime, lemon zest, lime zest, and any named fresh fruit or vegetable (e.g., tomato, potato, carrot, bell pepper, cucumber, spinach, mushroom, apple, berries) | None | — | Celery and celeriac match T-059, not this row. |
| T-078 | chicken, beef, pork, lamb, turkey, duck, veal (raw, unprocessed), and any named cut of them: steak, sirloin, ribeye, strip steak, filet, tenderloin, flank steak, skirt steak, brisket, chuck roast, short ribs, ground beef, ground turkey, ground pork, chicken breast, chicken thighs, drumsticks, wings, pork chops, pork loin, lamb chops, roast | None | — | Processed and cured meats (bacon, ham, sausage, deli meat, jerky) do not match this row. |
| T-079 | rice, white rice, brown rice, rice flour, quinoa, baking soda, yeast, vanilla extract | None | — | — |
| T-080 | corn, cornstarch, cornmeal, polenta, corn tortilla | None | — | Out of scope: corn. |
| T-081 | coconut, shredded coconut, coconut milk, coconut cream, coconut oil | None | — | Out of scope: coconut. Removed from the FDA tree nut list in January 2025; not on the EU/UK list. |
| T-082 | chestnuts | None | — | Out of scope: chestnut. Removed from the FDA tree nut list in January 2025; not on the EU/UK list. |
| T-085 | tomato paste, tomato puree, tomato pulp, tomato concentrate | None | — | US standard of identity (21 CFR 155.191): concentrated tomato liquid; optional lemon juice, concentrated lemon juice, or organic acids. Tomato sauce, ketchup, and salsa do not match this row; canned tomatoes match T-087. |
| T-086 | tomato juice, concentrated tomato juice | None | — | US standard of identity (21 CFR 156.145): unfermented liquid from mature tomatoes; may be seasoned with salt and acidified with organic acid. Vegetable juice blends do not match this row — they commonly contain celery. |

---

## Change Log

| Version | Change |
|---|---|
| 1.0 | Initial table, T-001 to T-084. |
| 1.1 | Added T-085 (tomato paste and puree), based on the US standard of identity. |
| 1.2 | Added peppercorn spellings to T-070 so whole peppercorns match one row deterministically. |
| 1.3 | Added T-086 (tomato juice, per 21 CFR 156.145), T-087 (canned tomatoes) and T-088 (stewed tomatoes) flagging celery, per 21 CFR 155.190, and T-089 (croutons) and T-090 (sausage) flagging the allergens a supplier label should be checked for. |
| 1.4 | Added named cuts of the raw meats to T-078 (steak, sirloin, brisket, chicken breast, pork chops, and so on). Edge testing showed "steak" landing in the review queue, which is noise rather than a finding. Processed and cured meats remain excluded. |

## Adding Rows

New rows come only from proposals a person has reviewed and approved (see `extras/research-mode.md`). The translator never adds rows. An approved row receives the next unused ID, is added under its allergen group, and increments the version. Existing IDs are never reused or renumbered, so citations in earlier outputs stay valid.
