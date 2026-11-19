# Greek `eStrong` Mapping Summary (`BDAG`, `LSJ`)

Reference: `000-TBESG - Translators Brief lexicon of Extended Strongs for Greek - STEPBible.org CC BY.txt` (`10,847` unique `eStrong#` entries).

## 1. Mapping Stages

1. **3-Tier Direct Greek Normalization**
   - Expanded `(...)` and `,`/`/` variants in both TBESG `Greek` and TSV `lexeme`.
   - Matched via `NFKC` exact (normalizing polytonic oxia/tonos) → `NFKC` lowercase → diacritic-stripped lowercase (`NFD` drop `Mn`).
2. **Morphological Variants & Disambiguator Stripping**
   - Stripped numeric disambiguators like `(2)`.
   - Matched voice alternations (`-ομαι` ↔ `-ω`, `-εομαι` ↔ `-εω`, `-αομαι` ↔ `-αω`, `-οομαι` ↔ `-οω`), `-υω` ↔ `-υμι`, nominal/adjectival endings (`-ον`/`-ος`/`-οι`, `-η`/`-α`), comparatives/superlatives (`-έστερος`/`-έστατος`/`-ότερος`/`-ότατος`), and `ληψ` ↔ `λημψ`.
3. **1-Edit Fuzzy Matching (`del1` Index)**
   - Matched lemmas ≥ 5 characters within Levenshtein distance ≤ 1 to resolve spelling variants and minor TBESG typos.
4. **Reverse-Mapping & Audit Cleanup**
   - Verified all `14` NT groups (`43` rows) that still had empty `eStrong` in `morphology.sqlite` against `000-TBESG` glosses/meanings, filled `morphology.sqlite` to **100.00%** (`138,013 / 138,013`), and reverse-mapped those `eStrong#`s back into `BDAG_original.tsv` (`+14` rows) and `LSJ_original.tsv` (`+6` rows).
   - Fixed `LSJ15883` (`ἀπόλλύω` → `G622`), cleaned `BDAG6596` (`πώς` → `G4458`), and stripped all leading zeros (`G0...` → `G...`).

## 2. Final Coverage

| File | Rows Mapped | Unique `eStrong#` Mapped | `eStrong#` Coverage | Remaining |
| --- | ---: | ---: | ---: | ---: |
| `BDAG_original.tsv` | 6,335 / 8,256 (76.7%) | 7,022 / 10,847 | 64.74% | 3,825 |
| `LSJ_original.tsv` | 10,741 / 140,969 (7.6%) | 10,373 / 10,847 | 95.63% | 474 |
| **Combined (≥ 1 TSV)** | — | **10,648 / 10,847** | **98.17%** | **199** |

## 3. Remaining Unmatched `eStrong#` (`199` entries)

### A. Standard NT Strong's (`G0001–G5999`, `36` entries)
Mostly suppletive/irregular verbs, multi-word phrases, and proper-name variants filed under another form in `BDAG`.

| eStrong# | Greek | Morph | Gloss |
| --- | --- | --- | --- |
| `G0291` | `Ἀμπλίας` | `N:N-M-P` | `Ampliatus` |
| `G0581` | `ἀπογενόμενος` | `G:V` | `to cease to be` |
| `G1003` | `Βοόζ` | `N:N-M-P` | `Boaz` |
| `G1490` | `εἰ δὲ μή(γε)` | `G:PRT-N` | `but if not` |
| `G1499` | `εἰ καί` | `G:CONJ` | `if also` |
| `G1509` | `εἰ μή τι` | `G:COND + G:PRT-N + G:CONJ` | `except what` |
| `G1527` | `εἷς καθ᾽ εἷς` | `G:ADV` | `one by one` |
| `G1536` | `εἴ τις` | `G:COND` | `if any` |
| `G1548` | `ἐκγαμίσκω` | `G:V` | `to give in marriage` |
| `G1672` | `Ἕλλην=Ἕλλην` | `N:N--LG` | `Greek, Gentile` |
| `G1673` | `Ἑλληνικός=Ἑλληνικός` | `N:A--LG` | `Greek` |
| `G1674` | `Ἑλληνίς=Ἑλληνίς` | `N:N-F-LG` | `Gentile` |
| `G1676` | `Ἑλληνιστί=Ἑλληνικός` | `G:ADV-T` | `Greek` |
| `G1691` | `ἐμέ` | `G:P-1` | `me` |
| `G1700` | `ἐμοῦ` | `G:P-1` | `of me` |
| `G1970` | `ἐπιπνίγω` | `G:V` | `to choke` |
| `G2076` | `ἐστί` | `G:V` | `he is` |
| `G2252` | `ἤμην` | `G:V` | `I was` |
| `G2254` | `ἡμῖν` | `G:P-1` | `to us` |
| `G2484` | `Ἰτουραΐα` | `N:N--L` | `Ituraea` |
| `G2499` | `Ἰωσή` | `N:N-M-P` | `Jose` |
| `G2721` | `κατεφίστημι` | `G:V` | `to attack` |
| `G3362` | `ἐὰν μή` | `G:COND + G:PRT-N` | `unless` |
| `G3363` | `ἵνα μή` | `G:ADV-N` | `lest/so that... not` |
| `G3427` | `μοί` | `G:P-1` | `to me` |
| `G3450` | `μοῦ` | `G:P-1` | `of me` |
| `G3603` | `ὅ ἐστι` | `G:V` | `which is` |
| `G3801` | `ο ων και ο ην και ο ερχομενος` | `G:T + G:V + G:CONJ + G:T + G:V + G:CONJ + G:T + G:V` | `was, is, will be` |
| `G4252` | `Πρίσκιλλα` | `N:N-F-P` | `Priscilla` |
| `G4809` | `συκομωραία` | `G:N-F` | `sycamore tree` |
| `G5025` | `ταύταις` | `G:PRT` | `hence` |
| `G5035` | `ταχύ` | `G:ADV` | `quickly` |
| `G5128` | `τούτους` | `G:D` | `these` |
| `G5224` | `ὑπάρχοντα` | `G:N-N` | `possession` |
| `G5502` | `χερουβίμ` | `N:N--T` | `Cherub` |
| `G5516` | `χξϛ` | `G:A` | `666` |

### B. Extended LXX / Apocrypha (`G6000–G9999`, `136` entries — first 40 shown)
Primarily unattested Septuagint transliterations of Hebrew/Aramaic words (`αγανώθ`, `Αθανιν`, `αιλάμ`, etc.) absent from `BDAG` and `LSJ`.

| eStrong# | Greek | Morph | Gloss |
| --- | --- | --- | --- |
| `G6135` | `Ἄδαρ` | `N:N` | `Adar` |
| `G6148` | `Αθανιν` | `N:N` | `Athanim` |
| `G6155` | `αιδέδ` | `G:N` | `acclamation` |
| `G6160` | `αιλ` | `N:N` | `a portal` |
| `G6162` | `αιλαμμών` | `N:N` | `pediment` |
| `G6249` | `αμασενίθ` | `N:N` | `octaves` |
| `G6251` | `αματταρί` | `N:N` | `a mark` |
| `G6331` | `ἀνελεημόνως` | `G:A` | `mercilessly` |
| `G6338` | `ἀνθ᾽ οὗ` | `` | `because of` |
| `G6339` | `ἀνθ᾽ ὧν` | `G:PREP` | `because of` |
| `G6516` | `απφουσώθ` | `G:A` | `for the sick` |
| `G6529` | `αριήλ` | `N:N` | `altar hearth` |
| `G6530` | `αριήλ` | `G:N` | `a lion-like person` |
| `G6532` | `ἀριώθ` | `N:N` | `herbs` |
| `G6633` | `αφφουσώθ` | `G:A` | `apart` |
| `G6634` | `ἀφφώ` | `G:INJ` | `now` |
| `G6636` | `ἄχει` | `N:N` | `reed-grass` |
| `G6637` | `αχούχ` | `N:N` | `a thorn-bush` |
| `G6640` | `βααλτάμ` | `N:N` | `master` |
| `G6665` | `βεδέκ` | `N:N` | `a breach` |
| `G6666` | `βεζέκ` | `N:N` | `lightning` |
| `G6668` | `βερσεχθάν` | `N:N` | `a chest` |
| `G6677` | `βιρά` | `N:N` | `a palace` |
| `G6717` | `γαρέμ` | `N:N` | `frame` |
| `G6718` | `γασβαρηνός` | `G:N-M` | `a treasurer` |
| `G6722` | `γεδδούρ` | `N:N` | `a troop` |
| `G6794` | `δεκαεπτά` | `` | `seventeen` |
| `G6898` | `διαφαύσκω` | `G:V` | `to show light through` |
| `G6977` | `δὺο δύο` | `G:A` | `two by two` |
| `G6989` | `εβδομηκονταδύο` | `` | `seventy-two` |
| `G6990` | `εβδομηκονταεπτά` | `` | `seventy-seven` |
| `G6991` | `εβδομηκονταπέντε` | `` | `seventy-five` |
| `G6992` | `εβδομηκοντατέσσαρες` | `G:A` | `seventy-four` |
| `G6993` | `εβδομηκοντατρεῖς` | `` | `seventy-three` |
| `G7019` | `ἐγκοίλια` | `G:A` | `the intestines` |
| `G7048` | `εἰ δὲ καί` | `` | `but if even` |
| `G7056` | `εἰκοσιέξ` | `` | `twenty-six` |
| `G7185` | `ελμωνί` | `N:N` | `a concealed place` |
| `G7192` | `εμμωνείμ` | `` | `such and such place` |
| `G7233` | `ενενηκονταοκτώ` | `` | `ninety-eight` |

### C. Additional LXX / Extended (`G10000+`, `37` entries)
Rare LXX/Apocrypha words and transliterations.

| eStrong# | Greek | Morph | Gloss |
| --- | --- | --- | --- |
| `G20393` | `ἐξηχευη` | `` | `stupes` |
| `G20613` | `κολοβόρινος` | `` | `slit-nosed` |
| `G20721` | `μετεξέτεροι` | `` | `a certain amount of` |
| `G21150` | `συγκαταγηράσκω` | `G:V` | `to age together` |
| `G21426` | `Αθιν` | `G:N-PRI` | `Jathin` |
| `G21428` | `Αμαζονεῖς` | `G:N-PRI` | `Alimazons` |
| `G21429` | `Αμηαχι` | `G:N-PRI` | `Ameachi` |
| `G21430` | `Αμμαδι` | `G:N-PRI` | `Ammadi` |
| `G21433` | `Αυσιτιδι` | `G:N-PRI` | `Ausis` |
| `G21434` | `Βααλιμαθ` | `G:N-PRI` | `Baalimath` |
| `G21436` | `Γαλαμααν` | `G:N-PRI` | `Galamaan` |
| `G21438` | `Γανοζα` | `G:N-PRI` | `Ganozae` |
| `G21440` | `Ἐλιὰβ` | `G:N-PRI` | `Eliab` |
| `G21441` | `Ἐλιὰκ` | `G:N-PRI` | `Eliac` |
| `G21442` | `Ελιαδαε` | `G:N-PRI` | `Eliadae` |
| `G21443` | `Ελφαλατ` | `G:N-PRI` | `Eliphalat` |
| `G21445` | `Εσελων` | `G:N-PRI` | `Eselom` |
| `G21446` | `Εωβης` | `G:N-PRI` | `Thobes` |
| `G21447` | `Θεησους` | `G:N-PRI` | `Theesus` |
| `G21449` | `Θεκω` | `G:N-PRI` | `Theco` |
| `G21451` | `Θιμαθ` | `G:N-PRI` | `Themath` |
| `G21452` | `Ιαναθα` | `G:N-PRI` | `Janathan` |
| `G21453` | `Ιεθερμαθ` | `G:N-PRI` | `Jethermath` |
| `G21454` | `Ιεσσιβαθ` | `G:N-PRI` | `Jessibath` |
| `G21455` | `Ισβακωμ` | `G:N-PRI` | `Isbacom` |
| `G21460` | `Λεασαμυς` | `G:N-PRI` | `Leasamys` |
| `G21461` | `Μακαλωθ·` | `G:N-PRI` | `Makeloth` |
| `G21465` | `Μεργαβ` | `G:N-PRI` | `Mergab` |
| `G21466` | `Μολχολ` | `G:N-PRI` | `Molchom` |
| `G21467` | `Ναγεδ` | `G:N-PRI` | `Naged` |
| `G21468` | `Ναημ` | `G:N-PRI` | `Namé` |
| `G21469` | `Οζαν` | `G:N-PRI` | `Hozan` |
| `G21471` | `Σαφ` | `G:N-PRI` | `Saph` |
| `G21474` | `Σερραν` | `G:N-PRI` | `Serran` |
| `G21499` | `Κεζιβ` | `G:N-PRI` | `Kezib` |
| `G21500` | `Αλιμαζονεῖς` | `G:N-PRI` | `Alimazonians` |
| `G21501` | `Βοσορρας` | `G:N-PRI` | `Bosorrha` |

---

## 4. Further Suggestions to Complete

1. **Resolve the Remaining Standard NT (`G0001–G5999`) Entries via `content` / `Morphology.lexicon` Bridge**:
   - Since `Morphology.lexicon` (`Mapping` table) already has **100.00%** `BDAG` coverage (`5,399 / 5,399`) for all NT `G...` entries (including suppletive/irregular forms like `G4280` `προερέω` → `BDAG6321, BDAG6362`, `G5237` `ὑπερείδω` → `BDAG7713`, `G0675` `Ἄππιος` → `BDAG1066`), syncing those verified `BDAG` IDs back to `BDAG_original.tsv` will bring `G0001–G5999` coverage to ~100%.
2. **Search `LSJ_original.tsv` `content` for Rare LXX Words (`G6000+` / `G10000+`)**:
   - Some LXX words not listed as main `LSJ` headwords appear inside `content` of a related root or compound entry.
3. **Link LXX Hebrew Transliterations (`G6000+`) to `TBESH`**:
   - Entries like `αγανώθ`, `αιλάμ`, and `Ἄδαρ` are transliterated Hebrew nouns/names that have no native Greek lexicon entry in `BDAG` or `LSJ`; they can be cross-linked to their Hebrew `eStrong#` in `000-TBESH` rather than forced into Greek lexicons.
