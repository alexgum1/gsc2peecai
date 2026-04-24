# GSC Filter Criteria

Rules for selecting which Google Search Console queries to track as Peec.ai prompts.

## Hard requirements (both must pass)

| Criterion | Value | Reason |
|-----------|-------|--------|
| Minimum word count | ≥ 5 words | Short queries are rarely conversational |
| Minimum impressions (90d) | ≥ 100 | Ensures real search demand |

## Relevance gate (at least one must pass)

| Criterion | Logic |
|-----------|-------|
| Contains a W-question word | Any word in the query matches the W_WORDS list (case-insensitive) |
| Long query | Word count ≥ 10 (conversational even without a W-word) |

## W_WORDS list (German)

```
was, wie, welche, welcher, welches, welchen, welchem,
warum, wo, wann, wer, wen, wem, wessen,
wohin, woher, womit, wozu, wofür, wobei, wodurch,
weshalb, weswegen, wieso, inwieweit,
worin, worauf, worüber, woran, wovor, wonach
```

## Python implementation

```python
W_WORDS = {
    'was','wie','welche','welcher','welches','welchen','welchem',
    'warum','wo','wann','wer','wen','wem','wessen','wohin','woher',
    'womit','wozu','wofür','wobei','wodurch','weshalb','weswegen',
    'wieso','inwieweit','worin','worauf','worüber','woran','wovor','wonach'
}

def qualifies(query: str, impressions: int) -> tuple[bool, str | None]:
    words = query.lower().split()
    if len(words) < 5 or impressions < 100:
        return False, None
    has_w = any(w in W_WORDS for w in words)
    is_long = len(words) >= 10
    if has_w or is_long:
        reasons = []
        if has_w: reasons.append('W-Frage')
        if is_long: reasons.append(f'{len(words)} Wörter')
        return True, ' + '.join(reasons)
    return False, None
```

## Deduplication

Before creating prompts, compare candidates against existing Peec.ai prompts (text match, case-insensitive, strip whitespace). Skip duplicates silently.

## Sorting

Sort candidates by impressions descending. This ensures the highest-traffic queries are created first if the 50-prompt limit is hit.

## Examples

| Query | Words | Impressions | W-word | Long | Pass? |
|-------|-------|-------------|--------|------|-------|
| was kostet ein [produkt] | 5 | 500 | ✓ was | – | ✅ W-Frage |
| welches [produkt] passt zu mir | 5 | 1.200 | ✓ welches | – | ✅ W-Frage |
| wie funktioniert [feature] genau | 5 | 800 | ✓ wie | – | ✅ W-Frage |
| welche größe brauche ich für [anwendung] | 6 | 900 | ✓ welche | – | ✅ W-Frage |
| wie schnell ist [produkt] im test | 6 | 600 | ✓ wie | – | ✅ W-Frage |
| [produkt-name] [modell] kaufen empfehlung | 5 | 80 | – | – | ❌ zu wenig Impressionen |
| [produkt] für anfänger test erfahrungen | 5 | 150 | – | – | ❌ kein W-Wort, < 10 Wörter |
| welches [produkt] | 2 | 1.500 | ✓ | – | ❌ zu kurz (< 5 Wörter) |
