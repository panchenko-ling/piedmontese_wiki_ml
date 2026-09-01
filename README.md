# Piedmontese–Italian Parallel Corpora and MT Evaluation

Resources for the paper "When Metrics Miss the Language: MT Evaluation for Piedmontese" (submitted to CLiC-it 2026)

## Contents

| File | Description | Size |
|------|-------------|------|
| `pmswiki_noleak.jsonl` | Cleaned Piedmontese Wikipedia dump (no overlap with mined MT dataset) | ~59k documents, ~4M tokens |
| `wiki_mined.csv` | Italian–Piedmontese parallel sentence pairs mined from Wikipedia via LaBSE alignment | 5304 pairs |
| `translations_merged.csv` | Italian–Piedmontese translation pairs from FLORES+ sentences with NLLB model outputs | 140 sentences |

## Dataset Details

### Piedmontese Wikipedia Corpus (`pmswiki_clean.jsonl`)
- **Source:** Piedmontese Wikipedia dump (01/02/2026)
- **Cleaning:** Articles were cleaned using `mwparserfromhell`: removed categories, file/image links, wiki markup, URLs, section headers, image caption directives, pixel dimensions, ISO language code references etc.
- **Format:** JSONL, one article per line with fields `id`, `title`, `text`
- **Note:** Deduplicated against the mined MT dataset
### Mined MT Dataset (`wiki_mined.csv`)
- **Source:** Aligned Piedmontese–Italian Wikipedia article pairs via Wikidata interlanguage links
- **Alignment:** LaBSE cosine similarity ≥ 0.85; sentences between 5–100 words
- **Columns:** `pms` (Piedmontese), `it` (Italian)

### Translated Pairs (`translations_merged.csv`)
- **Source sentences:** Italian from FLORES+ dev set
- **Reference:** Human translations from Vico & Libovický (2026)
- **MT outputs:** Three NLLB conditions — `NLLB-avg`, `NLLB-avg+MT`, `NLLB-avg+DAE+MT`
- **Columns:** `flores_id`, `source`, `reference`, `translations_NLLB-avg`, `translations_NLLB-avg+MT`, `translations_NLLB-avg+DAE+MT`
## Language
- **Piedmontese** (ISO 639-3: `pms`) — endangered Romance language of the Piedmont region in Italy
- **Italian** (ISO 639-3: `ita`)
## License
CC BY-SA 4.0
## Contact
Yulia Panchenko, yulia.panchenko@edu.unito.it
