# Our Family Recipes

A static site holding 67 family and collected recipes, with faceted browsing,
combined title-and-ingredient search, and quantity scaling.

Live at: `https://jdawg2you.github.io/OurFamilyRecipes/`

## Files

| File | What it is |
|---|---|
| `index.html` | The whole site — markup, styles, and logic in one file. No framework, no build step. |
| `recipes.json` | Every recipe. This is the data the site runs on. |
| `og-image.png` | The card image shown when the link is texted or posted. |
| `.nojekyll` | Tells GitHub Pages to serve files as-is. |

`index.html` fetches `recipes.json` at load. Edit the JSON, commit, and the site
updates — nothing to rebuild.

## Required: set your address before sharing the link

Link previews need a full web address; a relative path will not work. Near the top
of `index.html` there are **four** lines containing `YOUR-USERNAME`. Replace that
placeholder — and `family-recipes` if you named the repo something else — with your
real Pages address, keeping the trailing slash:

```html
<link rel="canonical" href="https://jsmith.github.io/family-recipes/">
<meta property="og:url"     content="https://jsmith.github.io/family-recipes/">
<meta property="og:image"   content="https://jsmith.github.io/family-recipes/og-image.png">
<meta name="twitter:image"  content="https://jsmith.github.io/family-recipes/og-image.png">
```

Commit, wait for the rebuild, then test the preview at
`https://www.opengraph.xyz` — paste your URL and it shows what the card will look
like. Messaging apps cache previews aggressively, so check there **before** you text
the link to anyone.

## Adding or editing a recipe

Each entry in `recipes.json` looks like this:

```json
{
  "id": "moms-rolls",
  "title": "Mom's Rolls",
  "collection": "family",
  "source": null,
  "yield": "24 rolls or 2 loaves",
  "yieldNum": 24,
  "yieldUnit": "rolls",
  "prep": null, "cook": "10 min", "total": null,
  "oven": "425°F",
  "course": ["Breads & Rolls"],
  "main": ["Flour & Yeast"],
  "season": ["Thanksgiving", "Christmas"],
  "method": ["Make-ahead"],
  "isBaked": true,
  "ingredients": [
    { "q": "5", "u": "cups", "n": "flour" },
    { "q": null, "u": null, "n": "Salt, to taste" }
  ],
  "steps": ["Mix 3 cups flour, sugar, salt, and yeast in a large bowl."],
  "notes": ["For bread, use 6–6 1/2 cups flour."]
}
```

Rules that matter:

- **`id`** must be unique. Lowercase, hyphens, no spaces.
- **`collection`** is `"family"` or `"collected"`. Family entries set `"source": null`
  and get the brick-red treatment; collected entries must carry a source string.
- **Ingredients split into `q` / `u` / `n`** — quantity, unit, name. This is what
  makes scaling work.
  - `q` is a **string**, so `"1 1/2"` and ranges like `"3 3/4-4"` survive intact.
  - `q: null` means **do not scale this line** — salt to taste, cooking spray,
    garnish, anything without a real measurement.
- **`isBaked: true`** triggers the warning that baking time and pan size don't scale.
- A trailing `(label)` on an ingredient name — `"tahini (dressing)"` — renders as a
  subheading, but only when 2+ lines in that recipe share the label.

The facet values are fixed lists near the top of the script in `index.html`
(`COURSES`, `MAINS`, `SEASONS`, `METHODS`). A value used in `recipes.json` that
isn't in those lists simply won't appear as a filter chip.

## Checking your JSON before you commit

A trailing comma or a missing quote will blank the page. Paste the file into any
JSON validator first, or run:

```
python3 -m json.tool recipes.json > /dev/null && echo OK
```

## Note on privacy

This site is public to anyone with the URL. No surname appears anywhere in it.
Recipe notes are written in a family voice — worth a read-through before publishing
if any of them name people you'd rather not name.
