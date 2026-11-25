# Uncensored JAV code index

A machine-readable index of **55,833 uncensored Japanese adult video releases** from 15 studios, plus the ID format of
each studio. Use it to recognise release IDs in file names, check that an ID exists, and map it to a page with full
English metadata on [JAV Mania](https://www.jav-mania.net/).

Snapshot: 2026-11-25. Source: [JAV Mania](https://www.jav-mania.net/), which keeps titles, descriptions, genres,
performer profiles and sample images up to date. The Japanese catalogue for censored releases is [AV品番](https://avhinban.com/).

## Studios and ID formats

| Studio | ID format | Example ID | Typical file name | Releases |
|---|---|---|---|---|
| [Caribbeancom](https://www.jav-mania.net/sites/caribbeancom/) | `MMDDYY-NNN` | `112426-001` | `carib-112426-001.mp4` | 6,081 |
| [Caribbeancom Premium](https://www.jav-mania.net/sites/caribbeanpremium/) | MMDDYY_NNN, or SKY-NNN-NN / RED-NNN-NN (older series) | `112426_001` | `caribpr 112426_001.mp4` | 6,192 |
| [1Pondo](https://www.jav-mania.net/sites/1pondo/) | `MMDDYY_NNN` | `112326_001` | `1pondo_112326_001.mkv` | 4,846 |
| [PacoPacoMama](https://www.jav-mania.net/sites/pacopacomama/) | `MMDDYY_NNN` | `112426_100` | `paco 112426_100.mp4` | 3,214 |
| [10Musume](https://www.jav-mania.net/sites/10musume/) | `MMDDYY_NN` | `112426_01` | `10mu-112426_01.mp4` | 3,409 |
| [Muramura](https://www.jav-mania.net/sites/muramura/) | `MMDDYY_NNN(N)` | `112126_1276` | `muramura 112126_1276.mp4` | 1,738 |
| [HEYZO](https://www.jav-mania.net/sites/heyzo/) | `NNNN` | `3970` | `HEYZO-3970.mp4, heyzo_hd_3970.mp4` | 3,447 |
| [Kin8tengoku](https://www.jav-mania.net/sites/kin8/) | `NNNN` | `4278` | `kin8tengoku-4278.mp4` | 3,074 |
| [Nyoshin](https://www.jav-mania.net/sites/nyoshin/) | nNNNN, or lesNNN for the lesbian series | `n3424` | `nyoshin n3424.mp4` | 3,693 |
| [Heydouga (H:G:M:O)](https://www.jav-mania.net/sites/hgmo/) | `NNNN` | `6608` | `hgmo 6608.mp4` | 3,928 |
| [H4610](https://www.jav-mania.net/sites/h4610/) | ki + YYMMDD, or series + number (ori1000, gol011) | `ki261027` | `h4610-ki261027.mp4` | 3,797 |
| [C0930](https://www.jav-mania.net/sites/c0930/) | ki + YYMMDD, or series + number (hitozuma0051, gol0001) | `ki261027` | `c0930-ki261027.mp4` | 3,734 |
| [H0930](https://www.jav-mania.net/sites/h0930/) | ki + YYMMDD, or series + number (ori1000, gol004) | `ki260920` | `h0930-ki260920.mp4` | 3,231 |
| [Pikkur](https://www.jav-mania.net/sites/pikkur/) | `NNNNN` | `04727` | `pikkur 04727.mp4` | 2,952 |
| [AV9898](https://www.jav-mania.net/sites/av9898/) | NNN or NNNN | `2948` | `av9898 2948.mp4` | 2,497 |

`MMDDYY` is the release date. The same `MMDDYY_NNN` ID can exist on 1Pondo, Caribbeancom Premium **and**
PacoPacoMama, so a bare `MMDDYY_NNN` only identifies a release together with the studio name.

A handful of legacy IDs do not follow these formats (listed below). [`data/studios.json`](data/studios.json) has the same table with an anchored regex for each ID, the aliases seen in
file names, and URL templates.

Legacy IDs: `1pondo/081007_00a`, `caribbeancom/022305-vip`, `caribbeancom/092904-h00`, `caribbeancom/vip_003`, `caribbeancom/vip_004`, `caribbeancom/vip_005`, `caribbeancom/vip_006`, `caribbeancom/vip_007`, `caribbeancom/vip_008`, `caribbeanpremium/030922-002`, `caribbeanpremium/030922-003`, `caribbeanpremium/062922-001`, `caribbeanpremium/062922-002`, `caribbeanpremium/090625-001`.

## Release index

[`data/uncensored_jav_index.csv`](data/uncensored_jav_index.csv) has one row per release:

| Column | Example |
|---|---|
| `studio` | `pacopacomama` |
| `content_id` | `101416_183` |
| `release_date` | `2016-10-14` |
| `duration_seconds` | `3640` |
| `performers_en` | `Akemi Honjo` (comma separated) |
| `performers_ja` | `本条朱美` |
| `url` | `https://www.jav-mania.net/movies/pacopacomama/101416_183/` |

Titles, descriptions and genres are not included. They are on each release page (`url`).

## Example

```python
import csv, json, re

studios = {s["studio"]: s for s in json.load(open("data/studios.json"))}
index = {(r["studio"], r["content_id"]): r for r in csv.DictReader(open("data/uncensored_jav_index.csv", encoding="utf-8"))}

def lookup(studio, content_id):
    if not re.match(studios[studio]["id_regex"], content_id):
        return None
    return index.get((studio, content_id))

print(lookup("pacopacomama", "101416_183")["url"])
```

Tools that already use JAV Mania: [Stash scraper](https://github.com/jav-mania/stash-scraper).

## License

Data: [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/). Please credit
"JAV Mania (https://www.jav-mania.net/)". Example code: MIT.
