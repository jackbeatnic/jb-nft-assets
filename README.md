# jb-nft-assets

**Metadata + media for Jack Beatnic NFT contracts (on-chain baseURI).**

This is a **separate GitHub repo** from the gallery website
(`jackbeatnic.github.io` / local `www/`).  
Separate repo = separate size/bandwidth limits; gallery can move to Cloudflare
without breaking mint URIs.

## Public vs private

- **URLs under GitHub Pages must be publicly fetchable** (OpenSea, wallets, indexers).
  That does **not** require a pretty gallery — only raw files at known paths.
- The **git repo** is typically public on free GitHub Pages; even a private repo
  still serves Pages content to anyone who has the URL.
- **Not for Google:** `robots.txt` disallows crawling; root `index.html` has
  `noindex`. Gallery SEO lives only on `jackbeatnic.github.io/` (www), which
  should `Disallow: /jb-nft-assets/` in its host `robots.txt`.

## Live base (GitHub Pages)

https://jackbeatnic.github.io/jb-nft-assets/

## Layout

→ **[STRUCTURE.md](./STRUCTURE.md)** — NS / FS / NJ × avalanche / base / polygon.

```text
media/{series}/{art_id}.jpg
meta/{chain}/{collection}/{token_id}
```

**Legacy NJ vol.2 (do not rename — already on-chain):**  
`meta/avalanche/nature-jam-2/` + `media/nature-jam-2/`

## Mint pipeline (monorepo)

```text
~/jb_nft/mintowanie/mint_gh_batch.py   # universal NS/FS/NJ
~/jb_nft/mintowanie/INSTRUKCJA.txt
~/jb_nft/mintowanie/lib_assets_paths.py
```

## Rules

1. **One artwork → one media file** (shared across chains).  
2. **One on-chain token → one meta file** under chain + collection.  
3. Meta `"image"` always points at `media/…`.  
4. Never store NFT mint media inside the gallery website repo.

## Arweave / IPFS later

Same tree → upload → one `baseURI` update per contract.  
See monorepo `mintowanie/HOSTING_META_MEDIA.md`.
