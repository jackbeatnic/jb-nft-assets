# jb-nft-assets

Metadata + media for Jack Beatnic NFT contracts.

**Not the gallery app** — gallery can move to Cloudflare; this repo stays as token URI host until IPFS/Arweave migration.

## URLs (GitHub Pages)

- Meta NJ vol.2: `https://jackbeatnic.github.io/jb-nft-assets/meta/avalanche/nature-jam-2/{id}`
- Media: `https://jackbeatnic.github.io/jb-nft-assets/media/nature-jam-2/{id}.jpg`

On-chain `baseURI` should match the meta pattern with `{id}`.

## Layout

```
meta/{chain}/{collection}/{token_id}   # JSON, no extension
media/{collection_or_series}/{id}.jpg  # shared across chains when same art
```

Triplets (AVAX/Base/Polygon): **one** media file, **three** meta files pointing at the same `image` URL.
