# Hebrew `eStrong` Mapping Summary (`BDBE`, `CHALOT`, `HALOT`)

Reference: `000-TBESH - Translators Brief lexicon of Extended Strongs for Hebrew - STEPBible.org CC BY.txt` (`9,345` unique `eStrong#` entries).

## 1. Mapping Stages

1. **Headword Expansion & Normalization**
   - Expanded `(...)` optional letters and `/` alternatives in TSV `lexeme`s, plus `,`-separated and `[...]` bracketed variants in TBESH `Hebrew`.
   - Removed `&zwj;`, applied Unicode `NFKC`, stripped cantillation (`U+0591`–`U+05AF`), meteg (`U+05BD`), and rafe (`U+05BF`), normalized `holam + vav` order, and converted internal maqaf (`־`) to space.
2. **3-Tier Direct Matching**
   - Exact vocalized → Vocalized phonetic skeleton (plene/defective `וֹ`/`ֹ`, `וּ`/`ֻ`, `ִי`/`ִ`, `ֵי`/`ֵ`, `ֶי`/`ֶ`, dagesh removed) → Consonantal (`א–ת`).
3. **Multi-Word Compound Resolution**
   - For multi-word TSV `lexeme`s where all component words (≥ 2 letters, excluding `׳` abbreviations) matched TBESH (stripping leading `הַ-` if needed), combined component `eStrong#`s in order.
4. **Morphological, Fuzzy, & `BDBE` Link Fallbacks**
   - Space/maqaf collapsing + final-letter normalization (`ךםןףץ` → `כמנפצ`).
   - Morphological variants (leading `הַ-`, directional `-ָה`, Aramaic `-אָ` ↔ `-ָה`, plural `-ִים`/`-וֹת`, feminine `-ִית`/`-ָה`/`-ת`).
   - 1-edit fuzzy consonantal matching (≥ 4 consonants) for minor spelling/source typos.
   - Embedded `S:H...` links in `BDBE_original.tsv` `content` for rows still empty.
5. **Reverse-Mapping & Audit Cleanup**
   - Verified all `336` OT groups (`4,036` rows) that still had empty `eStrong` in `morphology.sqlite` against `000-TBESH` glosses/meanings, filled `morphology.sqlite` to **100.00%** (`426,581 / 426,581`), and reverse-mapped those `eStrong#`s back into `CHALOT_original.tsv` (`+326` rows) and `HALOT_original.tsv` (`+324` rows).
   - Removed noisy `H9033` from `ו`/`וְ`/`וּ` rows (`BDBE2225`–`2226`, `CHALOT2095`–`2097`, `HALOT2183`–`2184`), fixed `לְמַעַן` (`H4616`), `הֵם` (`H1991, H1992`), `זָר` (`H2114`), `אֵלִיָּ֫הוּ` (`H452`), added `H7970` to `CHALOT8365`/`HALOT8526`, and stripped all leading zeros (`H0...` → `H...`).

## 2. Final Coverage

| File | Rows Mapped | Unique `eStrong#` Mapped | `eStrong#` Coverage | Remaining |
| --- | ---: | ---: | ---: | ---: |
| `BDBE_original.tsv` | 8,861 / 9,514 (93.1%) | 9,256 / 9,345 | 99.05% | 89 |
| `CHALOT_original.tsv` | 8,592 / 9,067 (94.8%) | 9,116 / 9,345 | 97.55% | 229 |
| `HALOT_original.tsv` | 8,660 / 9,286 (93.3%) | 9,113 / 9,345 | 97.52% | 232 |
| **Combined (≥ 1 TSV)** | — | **9,280 / 9,345** | **99.30%** | **65** |

## 3. Remaining Unmatched `eStrong#` (`65` entries)

### A. `H9xxx` Non-Lexical Prefixes, Suffixes, & Punctuation (`16` entries)
Not standalone dictionary headwords in `BDB`, `HALOT`, or `CHALOT`.

| eStrong# | Hebrew | Morph | Gloss |
| --- | --- | --- | --- |
| `H9013` | `ן/` | `Suffix` | `[emph.?]` |
| `H9014` | `-` | `Punct.` | `[-]` |
| `H9015` | `|` | `Punct.` | `[ ]` |
| `H9016` | `:` | `Punct.` | `[.]` |
| `H9019` | `/׆` | `Punct.` | `[¶]` |
| `H9021` | `ךָ` | `Ps2m` | `your` |
| `H9025` | `נוּ` | `Pp1c` | `our` |
| `H9026` | `כֶם` | `Pp2m` | `your` |
| `H9029` | `הֶן` | `Pp3f` | `their` |
| `H9031` | `ךָּ` | `Os2m` | `you` |
| `H9035` | `נוּ` | `Op1c` | `us` |
| `H9036` | `כֶם` | `Op2m` | `you` |
| `H9039` | `הֶן` | `Op3f` | `them` |
| `H9041` | `ךָ` | `Ss2m` | `you` |
| `H9043` | `נּוּ` | `Ss3m` | `he` |
| `H9045` | `נוּ` | `Sp1c` | `we` |

### B. Lexical / Compound Entries (`51` entries)
Almost all are **multi-word place/person names and Psalm titles** filed under a single component word in the lexicons (e.g., `אָבֵל`, `בֵּית`, `בַּעַל`, `חֲצַר`, `עֵין`, `רָמַת`), plus a few 2-edit/rare forms.

| eStrong# | Hebrew | Morph | Gloss |
| --- | --- | --- | --- |
| `H0025` | `אֲבִי גִבְעוֹן` | `N:N-M-P` | `Abi-gibeon` |
| `H0062` | `אָבֵל בֵּית־מֲעַכָה` | `N:N--L` | `Abel-beth-maachah` |
| `H0063` | `אָבֵל הַשִּׁטִּים` | `N:N--L` | `Abel-shittim` |
| `H0064` | `אָבֵל כְּרָמִים` | `N:N--L` | `Abel-keramim` |
| `H0065` | `אָבֵל מְחוֹלָה` | `N:N--L` | `Abel-meholah` |
| `H0066` | `אַבֵל מַיִם` | `N:N--L` | `Abel-maim` |
| `H0072` | `אֶבֶן הָעֵזֶר` | `N:N--L` | `Ebenezer` |
| `H0358` | `אֵילוֹן בֵּית חָנָן` | `N:N--L` | `Elonbeth-hanan` |
| `H0381` | `אִישׁ־חַיִל` | `H:N-M` | `hero` |
| `H0415` | `אֵל אֱלֹהֵי יִשְׂרָאֵל` | `N:N-M-P` | `El-Elohe-Israel` |
| `H0416` | `אֵל בֵּית־אֵל` | `N:N--L` | `El-bethel` |
| `H0439` | `אַלּוֹן בָּכוּת` | `N:N--L` | `Allon-bacuth` |
| `H0516` | `אַל תַּשְׁחֵת` | `H:N-M` | `Do Not Destroy` |
| `H0760` | `אֲרַם צוֹבָה` | `N:N--L` | `Aram-zobah` |
| `H0763` | `אֲרַם נַהֲרַיִם` | `N:N--L` | `Mesopotamia` |
| `H1044` | `בֵּית־עֵ֫קֶד` | `N:N--L` | `Beth-eked` |
| `H1045` | `בֵּית עַשְׁתָּרוֹת` | `N:N--L` | `Beth-ashtaroth` |
| `H1170` | `בַּעַל בְּרִית` | `N:N-M-T` | `Baal-berith` |
| `H1176` | `בַּעַל זְבוּב` | `N:N-M-T` | `Baal-zebub` |
| `H1181` | `בַּעֲלֵי בָּמוֹת` | `N:N--LG` | `Baal-bamothite` |
| `H1184` | `בַּעֲלֵי יְהוּדָה` | `N:N--L` | `Baale-judah` |
| `H1946` | `הוּךְ` | `A:V` | `to go` |
| `H1996` | `הֲמוֹן גּוֹג` | `N:N--L` | `Hamon-gog (Valley)` |
| `H2334` | `חַוּוֹת יָעִיר` | `N:N--L` | `Havvoth-jair` |
| `H2425` | `חָיַי` | `H:V` | `to live` |
| `H2578` | `חֲמַת צוֹבָה` | `N:N--L` | `Hamath-zobah` |
| `H2579` | `חֲמַת רַבָּה` | `N:N--L` | `Hamath the great` |
| `H3071` | `יְהֹוָה נִסִּי` | `N:N--L` | `YHWH/Jehovah-nissi` |
| `H3072` | `יְהֹוָה צִדְקֵנוּ` | `N:N-M-P` | `YHWH/Yahweh-tsidkenu` |
| `H3073` | `יְהֹוָה שָׁלוֹם` | `N:N--L` | `YHWH/Yahweh-shalom` |
| `H3074` | `יְהֹוָה שָׁמָּה` | `N:N--L` | `YHWH/Yahweh-shammah` |
| `H3128` | `יוֹנַת אֵלֶם רְחֹקִים` | `H:N` | `Dove on Far-off Terebinths` |
| `H3212` | `יָלַךְ` | `H:V` | `to walk` |
| `H3240` | `יָנַח` | `H:V` | `to rest` |
| `H3613` | `כָּלֵב אֶפְרָתָה` | `N:N--L` | `Caleb` |
| `H4036` | `מָגוֹר מִסָּבִיב` | `N:N-M-P` | ``Terror on Every Side`` |
| `H4965` | `מֶתֶג הָאַמָּה` | `N:N--L` | `Metheg-ammah` |
| `H5638` | `סְתָיו` | `H:N-M` | `winter` |
| `H5863` | `עִיֵּי הָעֲבָרִים` | `N:N--L` | `Iye-abarim` |
| `H5875` | `עֵין הַקּוֹרֵא` | `N:N--L` | `En-hakkore` |
| `H5878` | `עֵין חֲרֹד` | `N:N--L` | `Harod` |
| `H5883` | `עֵין רֹגֵל` | `N:N--L` | `En-rogel` |
| `H6255` | `עַשְׁתְּרֹת קַרְנַיִם` | `N:N--L` | `Ashteroth-karnaim` |
| `H6366` | `פֵּיָה` | `H:N-F` | `two-edged` |
| `H6549` | `פַּרְעֹה נְכֹה` | `N:N-M-P` | `Pharaoh-neco` |
| `H7025` | `קִיר חֶרֶשׂ` | `N:N--L` | `Kir-hareseth` |
| `H7434` | `רָמַת הַמִּצְפֶּה` | `N:N--L` | `Ramath-mizpeh` |
| `H7436` | `רָמָתַיִם צוֹפִים` | `N:N--L` | `Ramathaim-zophim` |
| `H7437` | `רָמַת לֶחִי` | `N:N--L` | `Ramath-lehi` |
| `H7741` | `שָׁוֵה קִרְיָתַיִם` | `N:N--L` | `Shaveh-kiriathaim` |
| `H7802` | `שׁוּשַׁן עֵדוּת` | `N:N-M-T` | `Shushan-eduth` |

---

## 4. Further Suggestions to Complete

1. **Map Multi-Word TBESH Compounds to Component-Word Lexicon Rows**:
   - Currently, multi-word resolution only ran from multi-word TSV `lexeme`s → TBESH. Running the reverse direction (splitting each remaining multi-word TBESH entry like `H0062` `אָבֵל בֵּית־מֲעַכָה` and appending `H0062` to its primary headword row `אָבֵל` or `בֵּית` in the TSVs, or verifying via `content` search) would cover nearly all of the remaining **51** `H0001–H8999` entries.
2. **Search TSV `content` for Compound Names**:
   - Many of the `51` compounds (e.g., `H0025` `אֲבִי גִבְעוֹן`, `H0439` `אַלּוֹן בָּכוּת`, `H5555` `סֶלַע הַמַּחְלְקוֹת`) appear verbatim inside the `content` HTML of their parent headword row.
3. **Keep `H9xxx` (`16` entries) Unmapped or Separate**:
   - Punctuation (`-`, `|`, `:`, `/׆`) and bound suffixes (`ךָ`, `נוּ`, `כֶם`, `הֶן`) have no corresponding lexicon entries and are best left unmapped in lexicon TSVs.
