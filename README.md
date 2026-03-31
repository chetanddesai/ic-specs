# Idle Champion Specialization Picker

Specialization picker for [Idle Champions of the Forgotten Realms](https://www.idlechampions.com/). Enter the number of relevant stacks/items for each specialization and the tool recommends the one that gives the highest damage boost.

**Live site:** https://chetanddesai.github.io/ic-specs/

## Bench

| Seat | Champion | Class | Data |
|------|----------|-------|------|
| 1 | [Anson](https://www.reddit.com/r/idlechampions/comments/1q6tyr4/year_9_champion_guide_anson_the_human_fighter/) | Human Fighter | Ready |
| 6 | [Lark](https://www.reddit.com/r/idlechampions/comments/1pdir3h/year_9_champion_guide_lark_the_tiefling_bard/) | Tiefling Bard | Ready |
| 8 | [Tess](https://www.reddit.com/r/idlechampions/comments/1lq6sdu/year_8_champion_guide_tess_the_wood_elf_rogue/) | Wood Elf Rogue | Ready |
| 9 | [King of Shadows](https://www.reddit.com/r/idlechampions/comments/1nvjj7p/year_9_champion_guide_king_of_shadows_the_human/) | Human Wizard | Ready |

## Adding Champion Data

Edit `data/champions.json`. Each champion entry has:

- `specializations` — array of specialization names (e.g. `["Pure of Heart", "Found Family", "Never Surrender"]`)
- `table` — object keyed by the input number, with values as an array of percentages (as raw numbers, no commas) matching the specializations order

Example for a 3-spec champion:

```json
{
  "name": "Anson",
  "specializations": ["Pure of Heart", "Found Family", "Never Surrender"],
  "table": {
    "2": [300, 406, 800],
    "3": [700, 1039, 2600]
  }
}
```

## Hosting

This is a static site served via GitHub Pages from the `main` branch root. No build step required.
