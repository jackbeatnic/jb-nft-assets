# jb-nft-assets — file layout (NS / FS / NJ × chains)

This repo **is not a gallery**. It only holds the files behind NFT URIs (meta JSON + media).  
The gallery (`jackbeatnic.github.io`, later Cloudflare) lives separately.

Pages root: `https://jackbeatnic.github.io/jb-nft-assets/`

---

## Core rule

| What | Where | Key |
|----|--------|--------|
| **1 JPG per artwork** | `media/{series}/{art_id}.jpg` | `art_id` = artwork number within the series (not the chain) |
| **1 JSON per token in a contract** | `meta/{chain}/{collection}/{token_id}` | on-chain `token_id` in **this** collection |
| JSON points to the JPG | `"image"` field | the same media URL for 2–3 chains |

**Triplets (the same art on AVAX + Base + Polygon):**

```text
media/nature_stories/6000.jpg          ← ONE file

meta/avalanche/nature_stories/6000     ← JSON, image → …/media/nature_stories/6000.jpg
meta/base/nature_stories/6000          ← same image URL
meta/polygon/nature_stories/6000       ← same image URL
```

Meta numbers may be the same "#6000" business-wise, but **the meta path always goes through chain + collection**.

---

## Directories

```text
jb-nft-assets/
  README.md
  STRUCTURE.md                 ← this file
  .nojekyll

  media/
    {series}/
      {art_id}.jpg             # art_id without leading zeros, e.g. 6000.jpg
      # optional: {art_id}.webp

  meta/
    {chain}/
      {collection}/
        {token_id}             # JSON WITHOUT the .json extension (ERC-1155 {id})
```

### Allowed `series` (media — shared across chains)

| series | Meaning |
|--------|-----------|
| `nature_jam` | Nature Jam (all vols / chains if the art is the same) |
| `nature_stories` | Nature Stories |
| `flower_stories` | Flower Stories |
| `based_ai` | Based AI (when needed) |
| `other` | exceptions |

### Allowed `chain` (meta)

| chain | Network |
|-------|------|
| `avalanche` | Avalanche C-Chain |
| `base` | Base |
| `polygon` | Polygon |
| `ethereum` | if ever needed |
| `sui` | when meta is hosted off-chain here (Sui usually uses a different pipeline) |

### Allowed `collection` (meta — folder = contract / logical slug)

Use **snake_case**, with stable ids matching the collection registry:

| collection | Example |
|------------|----------|
| `nature_jam_vol2` | NJ vol.2 Avalanche |
| `nature_jam` | NJ vol.1 |
| `nature_stories` | NS MAIN on a given chain |
| `flower_stories` | FS MAIN |
| `nature_stories_vol3` | when a vol has its own contract |

**Do not mix** files from two contracts in one `collection` folder.

---

## URLs (GitHub Pages)

```text
Meta:
https://jackbeatnic.github.io/jb-nft-assets/meta/{chain}/{collection}/{token_id}

Media:
https://jackbeatnic.github.io/jb-nft-assets/media/{series}/{art_id}.jpg
```

**On-chain baseURI** (one per contract):

```text
https://jackbeatnic.github.io/jb-nft-assets/meta/{chain}/{collection}/{id}
```

(SeaDrop / OpenSea: `{id}` placeholder in the string.)

---

## Mint batches (example workflow)

1. **Avalanche** — batch of 100: NS #5901–#6000  
   - `media/nature_stories/5901.jpg` … `6000.jpg`  
   - `meta/avalanche/nature_stories/5901` … `6000`  
   - mint on the AVAX contract  

2. **Base** — continues the business numbering, but **new token_ids** on the Base contract (or the same numbers if you keep them aligned)  
   - **DO NOT** copy the JPG again: the Base JSON only has  
     `"image": ".../media/nature_stories/5950.jpg"`  
   - `meta/base/nature_stories/{token_id}`  
   - mint on Base  

3. **Polygon** — same as above, a third JSON with the same `image`.

This way 50 new mints on Base ≠ 50 new megabytes if the art was already on AVAX.

---

## LEGACY — Nature Jam vol.2 (do not touch the paths)

Already on-chain and on Pages — **frozen URLs**:

```text
meta/avalanche/nature-jam-2/{token_id}     # note: hyphens in the folder name
media/nature-jam-2/{token_id}.jpg
baseURI: .../meta/avalanche/nature-jam-2/{id}
```

- `nature-jam-2` = historical folder name (OS slug).  
- New collections: **`nature_jam_vol2` snake_case** only for a **new** contract / new baseURI.  
- **Do not move** NJ vol.2 files to `nature_jam` without a baseURI migration (that would cause 404s).

Art mapping: for NJ2 `art_id` ≈ `token_id` (single chain). Offline source: `SUI/#{token_id - 41}`.

Inventory details are kept in the local mint pipeline (NJ vol.2 inventory).

---

## JSON — minimal schema

```json
{
  "name": "JB NJ #1056",
  "description": "…\nAvalanche Edition | Jack Beatnic 2026",
  "external_url": "https://jackbeatnic.github.io",
  "image": "https://jackbeatnic.github.io/jb-nft-assets/media/nature-jam-2/270.jpg"
}
```

Optional later: `attributes`, `animation_url`.  
For an Arweave migration: same folder layout; only the host in `image` + baseURI changes.

---

## Avoid

- JPGs in `meta/`  
- Meta from three chains in one directory without `{chain}/`  
- Duplicate `6000.jpg` in `media/avalanche/` and `media/base/`  
- Dropping assets into the **gallery** repo  
- Renaming the legacy `nature-jam-2` "for looks" without a baseURI plan  

---

## New batch checklist

1. Decide `series`, `art_id`, `chain`, `collection`, and the `token_id` range.  
2. Upload missing **media** (new art_ids only).  
3. Generate **meta** for that chain/collection.  
4. `git push` → wait for Pages.  
5. Mint on-chain (`mint_token` / batch) with a baseURI pointing to `meta/{chain}/{collection}/{id}`.  
6. Update the series inventory in the local mint pipeline.  
