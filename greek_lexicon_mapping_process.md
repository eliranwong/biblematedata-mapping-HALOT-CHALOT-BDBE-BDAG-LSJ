# Greek Lexicon Mapping Process (`BDAG`, `LSJ`) — Zero Strong's Dependency

This document describes the complete, Strong's-independent pipeline used to map all **5,399** Greek `G...` entries in [Morphology.lexicon](data/Morphology.lexicon) and all **138,013** Greek word occurrences in `morphology.sqlite` to their corresponding entries in:
- `BDAG_original.tsv` (`8,256` entries: `BDAG1`–`BDAG8256`)
- `LSJ_original.tsv` (`140,969` entries: `LSJ1`–`LSJ140969`)

---

## 1. Source Data & Schema

1. **ID Assignment in Lexicon CSVs**
   - Added a unique sequential ID column (`BDAG`, `LSJ`) and tab-separated header (`BDAG\tlexeme\tcontent`, `LSJ\tlexeme\tcontent`) at the beginning of each file (`BDAG1`–`BDAG8256`, `LSJ1`–`LSJ140969`).
2. **`Morphology.lexicon` (`Mapping` table)**
   - Added `BDAG` and `LSJ` (`TEXT DEFAULT ""`) columns to `Mapping`.
   - Extracted the Greek lemma for each `G...` `Topic` (`5,399` Greek entries; `Topic = 'info'` is metadata) from `<mlexeme><grk>...</grk></mlexeme>` in `Lexicon.Definition`.
3. **`morphology.sqlite` (`morphology` table)**
   - Added `BDAG`, `LSJ`, and `eStrong` (`TEXT DEFAULT ""`) columns at the end of `morphology`.
   - Populated `BDAG` and `LSJ` by joining `LexicalEntry` `G...` tokens against `Morphology.lexicon`'s `Mapping` table, with direct `morphology.Lexeme` fallback (zero Strong's lookup).
   - Populated `eStrong` via lexicon consensus (intersection of non-empty `eStrong` lists across mapped `BDAG`/`LSJ` rows, falling back to union), achieving `137,970 / 138,013` (`99.97%`), then verified the remaining `14` NT groups (`43` rows) against `000-TBESG` to reach `138,013 / 138,013` (`100.00%`) and reverse-mapped them into `BDAG_original.tsv` and `LSJ_original.tsv`.

---

## 2. Multi-Stage Greek Lexeme Mapping Pipeline

Each `G...` entry's `<mlexeme><grk>` string was matched against each CSV's `lexeme` (and `content` where needed) in strict precision order:

### Stage 1: Headword Expansion & Polytonic Unicode Normalization
Before indexing `BDAG_original.csv` and `LSJ_original.csv`:
- **Variant expansion**: expanded parenthetical `(...)` forms and split comma/slash-separated headword lists (e.g., `ἄρρην, ἄρσην` → `ἄρρην` and `ἄρσην`).
- **Unicode `NFKC` normalization**: unified polytonic Greek oxia (`U+1F71`, `U+1F7D`, etc.) with tonos (`U+03AC`, `U+03CE`, etc.), e.g., `Ἀαρών` ↔ `Ἀαρών`.
- **Three direct lookup tiers**:
  1. **Exact `NFKC`** (preserving case, breathings, and accents).
  2. **Lowercase `NFKC`** (`norm_lower`).
  3. **Diacritic-stripped lowercase** (`NFD` → drop combining marks `Mn` → `NFC` lowercase).

**Coverage after Stage 1 (direct Greek lexeme match alone):**
- `BDAG`: **5,165 / 5,400** (`95.65%`, including `info`)
- `LSJ`: **4,894 / 5,400** (`90.63%`, including `info`)

### Stage 2: Comma-Separated `<mlexeme><grk>` Splitting & Morphological Variants
- Split multi-form `<mlexeme><grk>` strings on `,` / `/` (e.g., `ἄρρην, ἄρσην`, `γαμίσκω, γαμίζω`, `γέννημα, γένημα`).
- Generated Greek morphological variants on diacritic-stripped lowercase forms:
  - **Voice alternations**: `-εομαι` ↔ `-εω`, `-αομαι` ↔ `-αω`, `-οομαι` ↔ `-οω`, `-ομαι` ↔ `-ω`, `-μαι` ↔ `-ω` (e.g., `ἀκαιρέω` ↔ `ἀκαιρέομαι`, `ἀμύνω` ↔ `ἀμύνομαι`).
  - **Nominal/adjectival endings**: `-ον` ↔ `-ος` / `-ης`, `-ος` ↔ `-οι` / `-ον` / `-ης`, `-η` ↔ `-α` / `-ος`.
  - **Comparatives / superlatives**: `-έστατος` / `-έστερος` → `-ής` / `-ώς`, `-ότατος` / `-ότερος` / `-ώτατος` / `-ώτερος` → `-ός` / `-ώς` (e.g., `ἀκριβέστατος` → `ἀκριβής`).
  - **Orthographic `ληψ` ↔ `λημψ`**: e.g., `ἀνάληψις` ↔ `ἀνάλημψις`.

### Stage 3: 1-Edit Fuzzy Matching (`BDAG`)
For remaining `BDAG` lemmas with ≥ 5 characters, matched against `BDAG` headwords within Levenshtein distance ≤ 1 (catching spelling variants such as `συκομωραία` ↔ `συκομορέα` and `Ἀχείμ` ↔ `Ἀχίμ`).

**Coverage after Stages 1–3:**
- `BDAG`: **5,364 / 5,400** (`99.33%`, `35` `G...` entries remaining + `info`)
- `LSJ`: **4,919 / 5,400** (`91.09%`)

### Stage 4: Disambiguator Stripping, `<wform>` / Attic-Koine Stem Equivalents, & `content` Search (`100.00%` `BDAG`)
For the final `35` `BDAG` entries (and applicable `LSJ` entries):
- **Strip numeric disambiguators**: `ἰός (2)` → `ἰός` (`BDAG3817`).
- **Attested `<wform><grk>` & `content` lookup**: matched `G675` `Ἄππιος` (`Ἀππίου` → `BDAG1066` `Ἀππίου φόρον`), `G3134` `μαρὰν ἀθά` (`BDAG4824` `μαράνα θά`), `G1600` `ἐκπετάζω` (`ἐξεπέτασα` → `BDAG2845`), `G2721` `κατεφίστημι` (`κατεπέστησαν` → `BDAG4232`), `G4567` `Σατανᾶς` (`BDAG6743` `σατάν`), `G5502` `χερουβίμ` (`BDAG8080` `Χερούβ`).
- **Verb `-μι` ↔ `-ω`, contracted, & suppletive stem equivalents**:
  - `G1166` `δεικνύω` → `δείκνυμι` (`BDAG1785`), `G3660` `ὄμνυμι` → `ὀμνύω` (`BDAG5409`), `G2910` `κρεμάω` → `κρέμαμαι, κρεμάννυμι` (`BDAG4505, BDAG4506`), `G581` `ἀπογενόμενος` → `ἀπογίνομαι` (`BDAG911`), `G1714` `ἐμπρήθω` → `ἐνέπρησε` (`BDAG2704`) / `ἐμπίμπρημι`, `G4280` `προερέω` → `προεῖπον, προλέγω` (`BDAG6321, BDAG6362`), `G5237` `ὑπερείδω` → `ὑπερεῖδον` (`BDAG7713`) / `ὑπεροράω`.
- **Adjective/adverb base lemmas**:
  - `G1452` `ἐγγύτερος` → `ἐγγύς` (`BDAG2220`), `G2082` `ἐσωτέρω` → `ἐσώτερος` (`BDAG3261`), `G4118` `πλεῖστος` → `πολύς` (`BDAG6193`), `G4210` `πορφύρεος` → `πορφυροῦς` (`BDAG6231`), `G4708` `σπουδαιοτέρως` → `σπουδαίως` (`BDAG6925`), `G5035` `ταχύ` → `ταχύς` (`BDAG7428`).

### Stage 5: Post-Mapping Audit, Disambiguation & Normalization
After populating `eStrong` in `morphology.sqlite` and auditing `LexicalEntry` vs. `eStrong` across all `138,013` NT rows:
- **Fixed `ἀπολλύω` (`90` rows)**: updated `BDAG` → `BDAG975` (`ἀπόλλυμι`) and `eStrong` → `G622` in `morphology.sqlite` and `Morphology.lexicon`, and cleaned `LSJ15883` (`ἀπόλλύω`) to `G622` (removing `G623`).
- **Disambiguated `ἱερός` (`72` noun vs. `2` adjective rows)**: updated noun occurrences (`MorphologyCode LIKE 'N-%'`, *temple*) to `BDAG3770`, `LSJ60447`, `G2411`, keeping `BDAG3772`, `LSJ60477`, `G2413` for the `2` adjective (*sacred*) rows.
- **Disambiguated `πως` (`110` enclitic vs. `8` interrogative rows)**: set enclitic `ADV` rows to `BDAG6596`, `LSJ111798`, `G4458`, and interrogative `ADV-I` rows to `BDAG6597`, `LSJ111799`, `G4459`; cleaned `BDAG6596` in `BDAG_original.tsv` to `G4458`.
- **Stripped leading zeros**: normalized `G0...` → `G...` across `morphology.sqlite` (`eStrong`) and both Greek TSVs (`BDAG`: `994` rows, `LSJ`: `992` rows).
- **Added `eStrongContextSpecific` column**: added `eStrongContextSpecific` at the end of `morphology`, filtering `eStrong` to keep only entries whose base number matches `LexicalEntry` whenever `LexicalEntry` matches at least one entry in `eStrong` (disambiguating homographs and removing LXX/extended duplicates when a single NT Strong's matches `LexicalEntry`; `100.00%` filled across `564,594` total rows, reducing multi-entry rows overall from `206,888` to `25,759`).

---

## 3. Final Greek Coverage

| Target | `BDAG` | `LSJ` | `eStrong` |
| --- | --- | --- | --- |
| `Morphology.lexicon` (`Mapping`, `5,399` `G...` entries) | **5,399 / 5,399 (100.00%)** | **4,965 / 5,399 (91.96%)** | — |
| `morphology.sqlite` (`morphology`, `138,013` Greek rows) | **138,013 / 138,013 (100.00%)** | **135,569 / 138,013 (98.23%)** | **138,013 / 138,013 (100.00%)** |

*(The remaining `434` `G...` entries unmapped in `LSJ`—`1.77%` of Greek NT occurrences—are predominantly biblical Hebrew/Aramaic proper names and Semitic transliterations such as `Ἀαρών`, `Ἀβραάμ`, and `Ἀδάμ`, which are not headwords in the classical `LSJ` lexicon.)*

