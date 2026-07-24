# jb-nft-assets

**Metadata + media for Jack Beatnic NFT contracts.**  
Not the gallery app — gallery can move to Cloudflare; this repo stays until IPFS/Arweave migration.

## Live base (GitHub Pages)

https://jackbeatnic.github.io/jb-nft-assets/

## Layout (read this)

→ **[STRUCTURE.md](./STRUCTURE.md)** — NS / FS / NJ × avalanche / base / polygon, shared JPG, parties.

```text
media/{series}/{art_id}.jpg
meta/{chain}/{collection}/{token_id}
```

**Legacy NJ vol.2 (do not rename):**  
`meta/avalanche/nature-jam-2/` + `media/nature-jam-2/`

## Quick rules

1. **One artwork → one media file** (even if minted on 3 chains).  
2. **One on-chain token → one meta file** under the right chain + collection.  
3. Meta `"image"` always points at `media/…`.  
4. Never store NFT media inside the gallery website repo.

## Arweave later

Same folder tree → upload → update each contract `baseURI` once.  
See monorepo `mintowanie/NJ_VOL2_INVENTORY.md` and `HOSTING_META_MEDIA.md`.
