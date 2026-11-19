# `Morphology.lexicon` — `Mapping` Table Documentation

This document explains how the `Mapping` table in [Morphology.lexicon](file:///home/eliran/dev/OpenHebrewBible/data/Morphology.lexicon) is structured and populated.

---

## 1. Table Schema & Scope

```sql
CREATE TABLE Mapping (
    Topic TEXT PRIMARY KEY,
    eStrongContextSpecific TEXT,
    BDBE TEXT,
    CHALOT TEXT,
    HALOT TEXT,
    BDAG TEXT,
    LSJ TEXT
);
```

- **Total rows**: **14,635** (`9,236` Hebrew/Aramaic `E...` entries + `5,399` Greek `G...` entries).
- The metadata row `Topic = 'info'` from `Lexicon` was removed from `Mapping`.

---

## 2. How Each Column Is Mapped

### Step 1: Lexicon-Headword Mapping (Zero Strong's Dependency)
Each `Topic` in `Morphology.lexicon`'s `Lexicon` table was first mapped to its corresponding lexicon IDs purely by matching its `<mlexeme><heb>...</heb></mlexeme>` (OT) or `<mlexeme><grk>...</grk></mlexeme>` (NT) lemma against the `lexeme` (and `content` where needed) columns of the five source lexicons:

- **Hebrew/Aramaic (`E...` → `BDBE`, `CHALOT`, `HALOT`)** — see [hebrew_lexicon_mapping_process.md](file:///home/eliran/dev/OpenHebrewBible/data/hebrew_lexicon_mapping_process.md):
  1. **Headword normalization & 4-tier direct match**: exact vocalized → phonetic vocalized skeleton (plene/defective vowels, dagesh removed) → consonantal → final-letter-normalized consonantal (`ךםןףץ` → `כמנפצ`).
  2. **Multi-word compound resolution**: component-word lookup for compound names/phrases.
  3. **Morphological & orthographic variants**: prefix `הַ-`, directional `-ָה`, Aramaic `-אָ` ↔ `-ָה`, feminine/plural endings, `יו-` ↔ `יהו-`, mater-less skeletons.
  4. **1-edit fuzzy matching & `content`/verse-reference lookup**: resolves remaining weak/inflected roots, source typos, and split compounds (e.g., `עֶגְלַת שְׁלִשִׁיָּה` → `HALOT6112, HALOT8591` and `CHALOT5927`).

- **Greek (`G...` → `BDAG`, `LSJ`)** — see [greek_lexicon_mapping_process.md](file:///home/eliran/dev/OpenHebrewBible/data/greek_lexicon_mapping_process.md):
  1. **Polytonic `NFKC` normalization & 3-tier direct match**: exact `NFKC` → lowercase `NFKC` → diacritic-stripped lowercase.
  2. **Morphological variants**: voice alternations (`-ομαι` ↔ `-ω`), `-υω` ↔ `-υμι`, nominal endings, comparatives/superlatives, `ληψ` ↔ `λημψ`, and numeric disambiguator stripping `(2)`.
  3. **1-edit fuzzy matching (`BDAG`) & `<wform>`/suppletive stem lookup**: resolves irregular verbs (`ἀπολλύω` → `BDAG975` `ἀπόλλυμι`) and variant spellings.

### Step 2: `eStrongContextSpecific` Derivation & Synchronization from `morphology.sqlite`
After populating `morphology.sqlite` (`morphology` table) with `BDBE`, `CHALOT`, `HALOT`, `BDAG`, `LSJ`, `eStrong`, and `eStrongContextSpecific`:
1. **`eStrong`** in [morphology.sqlite](file:///home/eliran/dev/OpenHebrewBible/data/morphology.sqlite) was computed from the TSV `eStrong` columns via lexicon consensus (intersection of non-empty `eStrong` lists across mapped lexicons, falling back to union, then audited against `000-TBESH` and `000-TBESG`).
2. **`eStrongContextSpecific`** in [morphology.sqlite](file:///home/eliran/dev/OpenHebrewBible/data/morphology.sqlite) filtered `eStrong` to keep only entries whose base `H`/`G` number matches `LexicalEntry` whenever `LexicalEntry` matches at least one entry in `eStrong` (preserving TBESH `a`/`b` subdivisions such as `H1254a, H1254b`, disambiguating homographs such as `אֵת` `H853` and `הַ` `H9009`, splitting `שָׁלֹשׁ` `H7969`/`H7970`, `ἱερός` `G2411`/`G2413`, `πως` `G4458`/`G4459`, and preserving compound components such as `עֶגְלַת שְׁלִשִׁיָּה` `H5697b, H7992`).
3. **`Mapping` table rebuild**: `Mapping` was cleared and repopulated by iterating over all `Topic` entries in `Lexicon` (`E...` and `G...`, excluding `info`) and aggregating the deduplicated union of `(eStrongContextSpecific, BDBE, CHALOT, HALOT, BDAG, LSJ)` from every matching `LexicalEntry` token in `morphology.sqlite`.

---

## 3. Final Verified Coverage (`14,635` rows)

| `Topic` Group | Rows | `eStrongContextSpecific` | `BDBE` | `CHALOT` | `HALOT` | `BDAG` | `LSJ` |
| --- | ---: | ---: | ---: | ---: | ---: | ---: | ---: |
| Hebrew/Aramaic (`E...`) | 9,236 | **9,236 (100.00%)** | **9,236 (100.00%)** | **9,236 (100.00%)** | **9,236 (100.00%)** | — | — |
| Greek (`G...`) | 5,399 | **5,399 (100.00%)** | — | — | — | **5,399 (100.00%)** | **4,965 (91.96%)** |

*(The `434` Greek `G...` entries without an `LSJ` mapping are primarily NT-only biblical Hebrew/Aramaic proper names and Semitic transliterations absent from the classical `LSJ` lexicon.)*

