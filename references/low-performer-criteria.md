# Low-Performer Criteria

Rules for identifying Peec.ai prompts that should be archived and replaced.

## Definition

A prompt is a **low-performer** when ALL three conditions are true:

| Condition | Threshold | Rationale |
|-----------|-----------|-----------|
| KI-Sichtbarkeit (visibility) | < 20% | Brand rarely appears in AI answers |
| Mention count | < 110 | Low absolute brand mentions |
| Not in GSC AI-Anfragen topic | — | Protect freshly created prompts |

The 110 mention threshold accounts for roughly 30 days × 3 active models × ~1.2 mentions/run. Prompts above this threshold are accumulating enough data to be meaningful even at low visibility.

## Additional archiving signals

Consider archiving (but confirm with user) if:

- Prompt has been running ≥ 14 days AND `visibility_total < 10` — the prompt may not be running properly
- Prompt is seasonal and clearly out of season (e.g., seasonal prompts in the wrong month)
- Prompt duplicates another prompt's intent (nearly identical text)

## What NOT to archive

- Prompts in the "GSC AI-Anfragen" topic created less than 14 days ago — give them time to accumulate data
- Prompts with high visibility (≥ 20%) even if mentions are low — they're performing
- Prompts the user explicitly wants to keep

## Data source

Fetch via `get_brand_report`:
```
project_id: <project>
start_date: 30 days ago
end_date: today
dimensions: ["prompt_id"]
filters: [brand_id = own_brand]
limit: 200
```

Key columns used: `prompt_id`, `visibility` (0–1), `mention_count`, `visibility_total`

## Example low-performers

| Prompt | Visibility | Mentions | Archive? |
|--------|-----------|---------|---------|
| [saisonales produkt] kaufen | 4% | 12 | ✅ Seasonal + low |
| [nischen-kategorie] für [anwendung] | 7% | 40 | ✅ Below both thresholds |
| welches [produkt] passt zu mir | 38% | 310 | ❌ High visibility |
| was kostet ein [produkt] | 17% | 90 | ❌ Recently created (< 14d) |
| [kategorie] kaufen in meiner nähe | 6% | 55 | ✅ Below both thresholds |

*Note: All visibility/mention values above are illustrative. Replace with real thresholds from your Peec.ai project.*

## Replacement logic

For each archived prompt, find the next-best GSC candidate that:
1. Passes the filter criteria (see `filter-criteria.md`)
2. Is not already active in Peec.ai
3. Was not previously archived in this session

Sort replacements by GSC impressions descending — highest traffic first.
