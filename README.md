# biblematedata-mapping-HALOT-CHALOT-BDBE-BDAG-LSJ

Headword and Extended Strong's (`eStrong`) mappings linking **BDBE**, **CHALOT**, **HALOT**, **BDAG**, and **LSJ** lexicon entries to BibleMate's original Hebrew/Aramaic and Greek morphology data.

## About the BibleMate AI Projects

This dataset extends [BibleMateData](https://github.com/eliranwong/biblematedata), which is part of the broader [BibleMate AI](https://github.com/eliranwong/biblemate) ecosystem, and is designed to work alongside:

- [BibleMate Web](https://github.com/eliranwong/biblemateweb) ([Web Demo](https://web.biblemate.ai))
- [BibleMate Agentic Workspace](https://github.com/eliranwong/biblemate-agentic-workspace)

The underlying Hebrew/Aramaic and Greek biblical materials are built on two open-source projects:

- [OpenHebrewBible](https://github.com/eliranwong/OpenHebrewBible)
- [OpenGNT](https://github.com/eliranwong/OpenGNT)

### Example Use Case: Lexicon Lookup

Developers can use these mappings to link morphological word occurrences directly to multiple standard lexicons (e.g., **BDBE**, **CHALOT**, **HALOT**, **BDAG**, **LSJ**) in a unified lexicon lookup interface:

<img width="2420" height="1366" alt="lexicon_lookup" src="https://github.com/user-attachments/assets/38bda3a8-d34a-4969-ad06-80171607c4bb" />

---

## Repository Structure

```text
.
├── README.md
├── data/
│   ├── Morphology.lexicon          # SQLite database with Lexicon and Mapping tables
│   └── morphology.tsv              # Word-level OT/NT morphology & lexicon mapping export (564,594 rows)
├── lexicon_mapping/
│   ├── BDBE.tsv                    # BDBE ID -> eStrong, lexeme (9,514 entries)
│   ├── CHALOT.tsv                  # CHALOT ID -> eStrong, lexeme (9,067 entries)
│   ├── HALOT.tsv                   # HALOT ID -> eStrong, lexeme (9,286 entries)
│   ├── BDAG.tsv                    # BDAG ID -> eStrong, lexeme (8,256 entries)
│   ├── LSJ.tsv                     # LSJ ID -> eStrong, lexeme (140,969 entries)
│   └── TBESH_TBESG_downloaded_2026-11-18/
│       ├── 000-TBESH - Translators Brief lexicon of Extended Strongs for Hebrew - STEPBible.org CC BY.txt
│       └── 000-TBESG - Translators Brief lexicon of Extended Strongs for Greek - STEPBible.org CC BY.txt
├── hebrew_lexicon_mapping_process.md
├── greek_lexicon_mapping_process.md
├── eStrong_Hebrew_mapping_summary.md
├── eStrong_Greek_mapping_summary.md
└── morphology_lexicon_mapping.md
```

---

## Data Files & Schemas

### `data/morphology.tsv`
Tab-separated export of the word-level `morphology` table (**564,594** data rows: **426,581** Hebrew/Aramaic OT + **138,013** Greek NT) with **16** columns:

| Column | Description |
| --- | --- |
| `WordID` | Unique word occurrence ID |
| `ClauseID` | Clause identifier |
| `Book`, `Chapter`, `Verse` | Biblical reference |
| `Word` | Inflected surface form |
| `Lexeme` | Lemma form |
| `Gloss` | English gloss |
| `LexicalEntry` | BibleMate lexical entry IDs (e.g. `E70001,H9003,` or `G3326,`) |
| `eStrong` | Consensus Extended Strong's number(s) across mapped lexicons |
| `eStrongContextSpecific` | Context-filtered `eStrong` aligned to `LexicalEntry` (preserving `a`/`b` subdivisions and homograph splits) |
| `BDBE`, `CHALOT`, `HALOT` | Mapped Hebrew/Aramaic lexicon entry IDs |
| `BDAG`, `LSJ` | Mapped Greek lexicon entry IDs |

### `data/Morphology.lexicon`
SQLite database containing:
- `Lexicon (Topic NVARCHAR(100), Definition TEXT)` — source lexicon definitions (`E...` and `G...` topics).
- `Mapping (Topic TEXT PRIMARY KEY, eStrongContextSpecific TEXT, BDBE TEXT, CHALOT TEXT, HALOT TEXT, BDAG TEXT, LSJ TEXT)` — **14,635** mapped entries (**9,236** Hebrew/Aramaic `E...` + **5,399** Greek `G...`).

### `lexicon_mapping/*.tsv`
Each lexicon mapping file has 3 tab-separated columns (`<LEXICON>`, `eStrong`, `lexeme`) with sequential IDs (`BDBE1`–`BDBE9514`, `CHALOT1`–`CHALOT9067`, `HALOT1`–`HALOT9286`, `BDAG1`–`BDAG8256`, `LSJ1`–`LSJ140969`). Note that the `content` column was stripped from these TSVs.

---

## Mapping Methodology

1. **Strong's-Independent Headword Mapping**
   - Each `E...` (Hebrew/Aramaic) and `G...` (Greek) entry in `Morphology.lexicon` was matched directly against the headwords (`lexeme` and, where needed, entry `content`) of the five lexicons without relying on Strong's numbers.
   - **Hebrew/Aramaic** ([hebrew_lexicon_mapping_process.md](hebrew_lexicon_mapping_process.md)): headword expansion (`(...)`, `/`), Unicode `NFKC` and vowel/dagesh normalization, 4-tier exact-to-consonantal matching, multi-word compound resolution, morphological/orthographic variants, and 1-edit fuzzy/content matching.
   - **Greek** ([greek_lexicon_mapping_process.md](greek_lexicon_mapping_process.md)): headword expansion, polytonic Unicode `NFKC` normalization, 3-tier exact-to-diacritic-stripped matching, voice/nominal/comparative morphological variants, 1-edit fuzzy matching, and suppletive stem lookup.

2. **`eStrong` & `eStrongContextSpecific` Derivation**
   - `eStrong` values were mapped from `000-TBESH` and `000-TBESG` ([eStrong_Hebrew_mapping_summary.md](eStrong_Hebrew_mapping_summary.md), [eStrong_Greek_mapping_summary.md](eStrong_Greek_mapping_summary.md)), reconciled via cross-lexicon consensus on `morphology`, and audited to **100%** word-level coverage.
   - `eStrongContextSpecific` filters `eStrong` against `LexicalEntry` to disambiguate homographs and preserve TBESH `a`/`b` subdivisions, then aggregates back into `Morphology.lexicon`'s `Mapping` table ([morphology_lexicon_mapping.md](morphology_lexicon_mapping.md)).

---

## Coverage Summary

| `Morphology.lexicon` (`Mapping`) | Rows | `eStrongContextSpecific` | `BDBE` | `CHALOT` | `HALOT` | `BDAG` | `LSJ` |
| --- | ---: | ---: | ---: | ---: | ---: | ---: | ---: |
| Hebrew/Aramaic (`E...`) | 9,236 | 9,236 (100.00%) | 9,236 (100.00%) | 9,236 (100.00%) | 9,236 (100.00%) | — | — |
| Greek (`G...`) | 5,399 | 5,399 (100.00%) | — | — | — | 5,399 (100.00%) | 4,965 (91.96%) |

*(The 434 Greek `G...` entries without an `LSJ` mapping are primarily NT-only biblical Hebrew/Aramaic proper names and Semitic transliterations absent from classical `LSJ`.)*

---

## Documentation

- [hebrew_lexicon_mapping_process.md](hebrew_lexicon_mapping_process.md) — Hebrew/Aramaic (`BDBE`, `CHALOT`, `HALOT`) headword mapping pipeline
- [greek_lexicon_mapping_process.md](greek_lexicon_mapping_process.md) — Greek (`BDAG`, `LSJ`) headword mapping pipeline
- [eStrong_Hebrew_mapping_summary.md](eStrong_Hebrew_mapping_summary.md) — Hebrew `eStrong` mapping against `000-TBESH`
- [eStrong_Greek_mapping_summary.md](eStrong_Greek_mapping_summary.md) — Greek `eStrong` mapping against `000-TBESG`
- [morphology_lexicon_mapping.md](morphology_lexicon_mapping.md) — `Morphology.lexicon` `Mapping` table schema, synchronization, and coverage

---

## Reference Attribution

Extended Strong's reference lexicons (`000-TBESH` and `000-TBESG`) in `lexicon_mapping/TBESH_TBESG_downloaded_2026-11-18/` are from [STEPBible.org](https://www.stepbible.org/) and licensed under **CC BY**.
