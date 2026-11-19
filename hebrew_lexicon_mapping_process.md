# Hebrew/Aramaic Lexicon Mapping Process (`BDB`, `CHALOT`, `HALOT`) — Zero Strong's Dependency

This document describes the complete, Strong's-independent pipeline used to map all **9,236** Hebrew/Aramaic `E...` entries in [Morphology.lexicon](file:///home/eliran/dev/OpenHebrewBible/data/Morphology.lexicon) and all **426,581** Hebrew/Aramaic word occurrences in [morphology.sqlite](file:///home/eliran/dev/OpenHebrewBible/data/morphology.sqlite) to their corresponding entries in:
- [BDBE_original.tsv](file:///home/eliran/dev/OpenHebrewBible/lexicon_mapping/BDBE_original.tsv) (`9,514` entries: `BDB1`–`BDB9514`)
- [CHALOT_original.tsv](file:///home/eliran/dev/OpenHebrewBible/lexicon_mapping/CHALOT_original.tsv) (`9,067` entries: `CHALOT1`–`CHALOT9067`)
- [HALOT_original.tsv](file:///home/eliran/dev/OpenHebrewBible/lexicon_mapping/HALOT_original.tsv) (`9,286` entries: `HALOT1`–`HALOT9286`)

---

## 1. Source Data & Schema

1. **ID Assignment in Lexicon TSVs**
   - Added a unique sequential ID column (`BDB`, `CHALOT`, `HALOT`) as the first column of each TSV (`BDB1...`, `CHALOT1...`, `HALOT1...`), yielding `ID\teStrong\tlexeme\tcontent`.
2. **`Morphology.lexicon` (`Mapping` table)**
   - Extracted the Hebrew/Aramaic lemma for each `Topic` (`E70001`–`E79236`, `9,236` entries out of `14,636` total `Topic` rows) from `<mlexeme><heb>...</heb></mlexeme>` in `Lexicon.Definition`.
   - Created `Mapping (Topic TEXT PRIMARY KEY, BDBE TEXT, CHALOT TEXT, HALOT TEXT)` (initially `BDB`, renamed to `BDBE` with `BDBE*` IDs).
3. **`morphology.sqlite` (`morphology` table)**
   - Added `BDBE`, `CHALOT`, `HALOT` (`TEXT`) columns (initially `BDB`, renamed to `BDBE` with `BDBE*` IDs) and later `eStrong` at the end of `morphology`.
   - Populated `BDBE`, `CHALOT`, `HALOT` solely by joining `LexicalEntry` `E...` tokens against `Morphology.lexicon`'s `Mapping` table (no `H...` Strong's numbers used).
   - Populated `eStrong` via lexicon consensus (intersection of non-empty `eStrong` lists across mapped `BDBE`/`HALOT`/`CHALOT` rows, falling back to union), achieving `422,545 / 426,581` (`99.05%`), then verified the remaining `336` OT groups (`4,036` rows) against `000-TBESH` to reach `426,581 / 426,581` (`100.00%`) and reverse-mapped them into the TSVs.

---

## 2. Multi-Stage Lexeme Mapping Pipeline

Each `E...` entry's `<mlexeme><heb>` string was matched against each TSV's `lexeme` (and `content` where needed) in strict precision order, stopping at the first stage that produced a match:

### Stage 1: TSV Headword Expansion & Normalization
Before indexing each TSV's `lexeme` column:
- **Parenthetical variants `(...)`**: expanded both with and without the parenthesized characters (e.g., `דִּשֹׁ(ֹו)ן` → `דִּשֹׁן` and `דִּשֹֹׁון`).
- **Slash variants `/`**: split on `/` and expanded suffix alternations (e.g., `אִגְּרָה/א` → `אִגְּרָה`, `אִגְּרָא`; `בִּ/בַּ/בְּ` → `בִּ`, `בַּ`, `בְּ`).
- **HTML & punctuation cleanup**: removed `&zwj;`, applied Unicode `NFKC` normalization (converting presentation forms like `שׁ`/`שׂ` to `שׁ`/`שׂ`), stripped cantillation (`U+0591`–`U+05AF`), meteg (`U+05BD`), and rafe (`U+05BF`), normalized `holam + vav` order (`\u05b9\u05d5` → `\u05d5\u05b9`), stripped boundary punctuation (`־׳·()[] `), and converted internal maqaf (`־`) to space.

### Stage 2: Direct Lexeme Matching (4-Tier Exact → Consonantal)
1. **Exact vocalized (`clean_base`)**: matches exact consonants + vowels + sin/shin dots.
2. **Phonetic vocalized skeleton (`to_phon`)**: normalizes plene/defective vowels (`וֹ`/`ֹ`, `וּ`/`ֻ`, `ִי`/`ִ`, `ֵי`/`ֵ`, `ֶי`/`ֶ`) and removes dagesh (`U+05BC`).
3. **Consonantal (`to_cons`)**: Hebrew letters `א–ת` and spaces only.
4. **Final-letter-normalized consonantal (`to_cons_nf`)**: maps final letters `ךםןףץ` → `כמנפצ` so suffix-stripped stems match final-letter headwords.

**Coverage after Stage 2 (direct lexeme match alone):**
- `BDB`: **8,790 / 9,236** (`95.17%`)
- `CHALOT`: **8,898 / 9,236** (`96.34%`)
- `HALOT`: **8,881 / 9,236** (`96.16%`)

### Stage 3: Multi-Word Compound Resolution
For multi-word `<mlexeme><heb>` entries (primarily compound place/person names like `בֵּית אֵל`, `בֵּית לֶחֶם`, `קָדֵשׁ בַּרְנֵעַ`) that do not have a single combined row in a given lexicon:
- Split on whitespace into component words (≥ 2 consonants each).
- Look up each component word via Stage 2 (and Stage 4 morphological variants) and combine all matching lexicon IDs in order, deduplicated.

### Stage 4: Morphological & Orthographic Variant Normalization
For remaining single-word lemmas, generated ordered consonantal variants (using `ךםןףץ` → `כמנפצ` normalization):
- **Prefix `הַ-` / article**: strip leading `ה` (`הַמֹּלֶכֶת` → `מֹלֶכֶת`) or prepend `ה` (`לֵזוּ` → `הַלֵּזוּ`).
- **Directional `-ָה` / Aramaic `-אָ`**: strip trailing `ה` (`יָהְצָה` → `יַהַץ`, `שַׁחֲצִימָה` → `שַׁחֲצִים`) or swap trailing `ה` ↔ `א`.
- **Feminine / plural**: swap `-ת` ↔ `-ה` (`מַצֶּבֶת` → `מַצֵּבָה`), strip plural `-ים` / `-ות` (and try `-י` / `-ה` stems, e.g., `לוּדִים` → `לוּדִי`), strip `-ית` → `-י`, or add `-ים` (`עֲדָשָׁה` → `עֲדָשִׁים`).
- **Theophoric `יו-` ↔ `יהו-`**: e.g., `יְהֹוסֵף` ↔ `יֹוסֵף`.
- **Mater-less consonantal (`to_cons_nomater`)**: for words with ≥ 4 consonants, strip non-initial `ו` and `י` (e.g., `קִמָּשֹׂון` → `קִמֹּשׂ`).

### Stage 5: 1-Edit Fuzzy Match & `content` HTML Search
For lemmas with ≥ 4 consonants still unmatched (typically OCR/source typos or minor consonantal differences):
- **Levenshtein distance ≤ 1** against TSV `lexeme` consonantal skeletons (e.g., `אֲבַטִּיחַ` ↔ `CHALOT` `אֲבַטִּיתַ`, `גְּאוּאֵל` ↔ `HALOT` `עגּאוּאֵל`).
- **`content` HTML lookup**: search TSV `content` for `S:<heb>` or `><heb><` links.

**Coverage after Stages 1–5:**
- `BDB`: **9,210 / 9,236** (`99.72%`, `26` remaining)
- `CHALOT`: **9,229 / 9,236** (`99.92%`, `7` remaining)
- `HALOT`: **9,230 / 9,236** (`99.94%`, `6` remaining)

### Stage 6: Attested `Word` Form & Verse-Reference Lookup (`100.00%`)
For the final `26` (`BDB`), `7` (`CHALOT`), and `6` (`HALOT`) entries—mostly 2-edit morphological/vowel differences (`אַשְׁמֹרֶת` → `אַשְׁמוּרָה`, `תַּעֲרוּבֹות` → `תַּעֲרֻבָה`, `אֲדַרְכֹּון` → `אֲדַרְכֹנִים`), weak/inflected roots (`יָלַהּ` → `תָּלָה`, `זחח` → `זוח`, `יסך` → `סוךְ`, `ישׁם` → `שׁמם`, `ישׂם` → `שׂים`), Aramaic emphatic endings (`שׁוּשַׁנְכָי` → `שׁוּשַׁנְכָיֵא`, `נֶבְרְשָׁה` → `נֶבְרַשְׁתָּא`), or split compounds (`הַחִירֹות` → `פִּי הַחִירֹת`, `פִּיָּה` → `יְפֵיפִיָּה`):
- Queried [morphology.sqlite](file:///home/eliran/dev/OpenHebrewBible/data/morphology.sqlite) for each `E...` entry's attested biblical `Word` and `Book Chapter:Verse` (`<BHS>` ref in TSV `content`).
- Matched the exact lexicon entry discussing that verse/form and recorded the mapping in `Mapping`, then re-synced `morphology.sqlite`.

### Stage 7: Post-Mapping Audit, Disambiguation & Normalization
After populating `eStrong` in `morphology.sqlite` and auditing `LexicalEntry` vs. `eStrong` across all `426,581` OT rows:
- **Replaced invalid `H9000` (`51,003` `וְ` rows)**: updated both `LexicalEntry` and `eStrong` to `H9001` when `Word` contains patach (`וַ`/`וַֽ`, `16,100` rows) and `H9002` for all other conjunction forms (`וְ`, `וּ`, `וָ`, `וִ`, `34,903` rows); removed noisy `H9033` (`וֹ` "him") from `ו`/`וְ`/`וּ` rows in `BDBE_original.tsv`, `CHALOT_original.tsv`, and `HALOT_original.tsv`.
- **Fixed homograph/fuzzy mismatches**: `לְמַעַן` (`272` rows → `HALOT4936`, `H4616`), `הֵם` (`269` rows → `H1992` in DB; `H1991, H1992` in TSVs), `זָר` (`70` rows → `H2114`), and `אֵלִיָּהוּ` (`63` rows → `BDBE454`, `H452`, keeping `BDBE456` `אֱלִיהוּ` as `H453`).
- **Split `שָׁלֹשׁ` by number**: updated `171` plural `שְׁלֹשִׁים` (*thirty*) rows to `eStrong = 'H7970'` and `BDBE = 'BDBE8780'` (and `CHALOT8365`/`HALOT8526` to `H7969, H7970`).
- **Aligned `LexicalEntry`**: updated `הִנֵּה` (`219` rows: `H2005` → `H2009`) and `אֵילָם` (`34` rows: `H197` → `H361`).
- **Stripped leading zeros**: normalized `H0...` → `H...` across `morphology.sqlite` (`eStrong`) and all three Hebrew TSVs (`BDBE`: `1,002` rows, `CHALOT`: `1,000` rows, `HALOT`: `1,023` rows).
- **Added `eStrongContextSpecific` column**: added `eStrongContextSpecific` at the end of `morphology`, filtering `eStrong` to keep only entries whose base number matches `LexicalEntry` whenever `LexicalEntry` matches at least one entry in `eStrong` (preserving TBESH `a`/`b` subdivisions such as `H1254a, H1254b` while disambiguating homographs such as `אֵת` `H853` and `הַ` `H9009`; `100.00%` filled across `564,594` total rows, reducing multi-entry rows overall from `206,888` to `25,759`).
- **Refined multi-word compound names in `eStrongContextSpecific` / `eStrong` / TSVs**:
  - Restored missing `LexicalEntry` components for `גֵּיא מֶלַח` (`2` rows → `H1516, H4417` in both `eStrong` and `eStrongContextSpecific`, replacing `H1341`; also filled `HALOT1572` `גַּיְ` → `H1516`), `בֵּית עֶדֶן` (`H1004a, H1004b, H5730`), `בֵּית הַגִּלְגָּל` (`H1004a, H1004b, H1537`), and `בֵּית עֵקֶד הָרֹעִים` (`H1004a, H1004b, H7462`).
  - Updated `עֶגְלַת שְׁלִשִׁיָּה` (`2` rows: Isa 15:5, Jer 48:34) to `HALOT6112, HALOT8591` (dropping `HALOT6111` `עֲגָלָה` *cart*) and `H5697b, H7992` in `eStrong`, `eStrongContextSpecific`, and `CHALOT5927`.

---

## 3. Final Coverage

| Target | `BDBE` | `CHALOT` | `HALOT` | `eStrong` |
| --- | --- | --- | --- | --- |
| `Morphology.lexicon` (`Mapping`, `9,236` `E...` entries) | **9,236 / 9,236 (100.00%)** | **9,236 / 9,236 (100.00%)** | **9,236 / 9,236 (100.00%)** | — |
| `morphology.sqlite` (`morphology`, `426,581` Hebrew/Aramaic rows) | **426,581 / 426,581 (100.00%)** | **426,581 / 426,581 (100.00%)** | **426,581 / 426,581 (100.00%)** | **426,581 / 426,581 (100.00%)** |

