---
name: add-champion
description: >-
  Add a new Idle Champions champion to the site. Use when the user provides a
  Reddit guide URL, seat number, and specialization data table for a champion
  to add.
---

# Add Champion

Add a new champion's specialization data to the ic-specs site.

## Required Inputs

1. **Guide URL** — Reddit link (e.g. `https://www.reddit.com/r/idlechampions/comments/.../year_N_champion_guide_NAME_the_RACE_CLASS/`)
2. **Seat number** — integer 1–12
3. **Data table** — pasted text with specialization names as column headers and rows of `Number | Spec1% | Spec2% | Spec3%`

## Inferred Inputs

- **Name** — extracted from URL slug (e.g. `flint_the_dwarven_fighter` → "Flint")
- **Class** — extracted from URL slug (e.g. `flint_the_dwarven_fighter` → "Dwarven Fighter")
- **Default** — `false` unless user explicitly says otherwise

## Workflow

### 1. Parse the URL slug

Extract name and class from the guide URL path segment:

```
year_9_champion_guide_flint_the_dwarven_fighter
                      ^^^^^     ^^^^^^^^^^^^^^^
                      name      class (title-cased, spaces replacing underscores)
```

### 2. Parse the data table

- Strip commas from numbers (`1,039` → `1039`)
- Strip `%` suffix
- `NA` or empty cell → `null`
- Row keys with trailing `*` (e.g. `14*`) → strip asterisk, use plain integer
- Double `%%` typo → treat as single `%`
- Result: integer arrays (or null) indexed by specialization order

### 3. Add entry to `data/champions.json`

Append a new object to the `champions` array:

```json
{
  "name": "Flint",
  "seat": 9,
  "default": false,
  "class": "Dwarven Fighter",
  "guide_url": "https://www.reddit.com/r/idlechampions/comments/1vglkf0/year_9_champion_guide_flint_the_dwarven_fighter/",
  "specializations": ["Spec A", "Spec B", "Spec C"],
  "table": {
    "2": [300, 406, 800],
    "3": [700, 1039, 2600]
  }
}
```

Schema rules:
- `table` keys are stringified integers
- `table` values are arrays of raw integers matching specializations order
- Use `null` for unavailable values (NA)

### 4. Add row to `README.md` bench table

Insert a new row in seat-number order:

```markdown
| <seat> | [<Name>](<guide_url>) | <Class> | <Yes/No> | Ready |
```

Columns: Seat, Champion (linked to guide), Class, Default, Data

### 5. Commit

Message format:

```
Add <Name> (Seat N) specialization data
```
