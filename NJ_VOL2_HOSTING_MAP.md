# NJ vol.2 — mapa hostingu (IPFS vs GH) pod scalenie na Arweave
Wygenerowano automatycznie. Pełna tabela: `NJ_VOL2_HOSTING_MAP.csv`
Tokenów w spisie: **680** (id 1–680)
## Meta (JSON) — wszystkie dziś
- **Host:** GitHub Pages `jb-nft-assets`
- **baseURI on-chain:** `https://jackbeatnic.github.io/jb-nft-assets/meta/avalanche/nature-jam-2/{id}`
- **Ścieżka:** `meta/avalanche/nature-jam-2/{token_id}`

## Obrazy — podział
| image_host | count |
|---|---:|
| GH_jb-nft-assets | 411 |
| IPFS_Pinata_or_legacy | 269 |

Pliki JPG fizycznie w GH media/: **411**
JSON z `image` = IPFS CID (bez lokalnego JPG w assets): **268** (era B głównie; #270 ma już też JPG na GH)

## Era mintu
| era | tokeny | count |
|---|---|---:|
| A_early_s30 | 1–41 | 41 |
| B_pinata_s3000 | 42–269 | 228 |
| C_os_selfmint_s3000 | 270–270 | 1 |
| D_gh_assets_s3000 | 271–680 | 410 |

## Jak scalić na Arweave
1. Dla wierszy z `gh_media_exists=true` → upload `media/nature-jam-2/{id}.jpg` na AR.
2. Dla wierszy z samym IPFS → `ipfs get` z `image_uri` (lub z offline `SUI/` / `przygotowane/`) → AR.
3. Przepisz wszystkie meta JSON: `image` → AR URL; zachowaj `{token_id}` w nazwie pliku meta.
4. `mint_token.py --tylko-base-uri --base-uri '<AR_BASE>/{id}'`.

## Kolumny CSV
`token_id, name, era, meta_host, image_host, image_uri, gh_media_file, sui_source_n, arweave_merge_note, …`
