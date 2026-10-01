# NJ vol.2 — hosting map (IPFS vs GH) for merging to Arweave
Auto-generated. Full table: `NJ_VOL2_HOSTING_MAP.csv`
Tokens listed: **680** (ids 1–680)
## Meta (JSON) — all of them today
- **Host:** GitHub Pages `jb-nft-assets`
- **On-chain baseURI:** `https://jackbeatnic.github.io/jb-nft-assets/meta/avalanche/nature-jam-2/{id}`
- **Path:** `meta/avalanche/nature-jam-2/{token_id}`

## Images — split
| image_host | count |
|---|---:|
| GH_jb-nft-assets | 411 |
| IPFS_Pinata_or_legacy | 269 |

JPG files physically in GH media/: **411**
JSON with `image` = IPFS CID (no local JPG in assets): **268** (mostly era B; #270 also has a JPG on GH)

## Mint era
| era | tokens | count |
|---|---|---:|
| A_early_s30 | 1–41 | 41 |
| B_pinata_s3000 | 42–269 | 228 |
| C_os_selfmint_s3000 | 270–270 | 1 |
| D_gh_assets_s3000 | 271–680 | 410 |

## How to merge to Arweave
1. For rows with `gh_media_exists=true` → upload `media/nature-jam-2/{id}.jpg` to AR.
2. For IPFS-only rows → `ipfs get` from `image_uri` (or from the offline `SUI/` / `prepared/` folders) → AR.
3. Rewrite all meta JSON: `image` → AR URL; keep `{token_id}` as the meta file name.
4. Update the contract baseURI to `'<AR_BASE>/{id}'` (base-URI-only mint tool run).

## CSV columns
`token_id, name, era, meta_host, image_host, image_uri, gh_media_file, sui_source_n, arweave_merge_note, …`
