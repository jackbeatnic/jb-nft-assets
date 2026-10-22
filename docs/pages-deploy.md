# GitHub Pages deploy

Pages for this repo is a legacy site: source branch `main`, path `/`, custom
domain `jackbeatnic.art` (same files as `https://jackbeatnic.github.io/jb-nft-assets/`).
`.nojekyll` is on `main`, so extensionless metadata files are copied as-is.

SeaDrop baseURI stays on this host (`.../meta/{chain}/{collection}/{id}`, no
`.json`). Do not change it on-chain to work around a slow publish.

## 2026-10-22 avalanche flower_stories 4967–5026

Both batches were already on `main`:

- `0c2e9b6` avalanche_flower_stories assets 4967-4996 (30)
- `79fd927` avalanche_flower_stories assets 4997-5026 (30)

`raw.githubusercontent.com` returned HTTP 200 immediately. Pages still
returned HTTP 404 above token 4966 until the Pages run finished. That was not
a missing commit. Run
[39843484211](https://github.com/jackbeatnic/jb-nft-assets/actions/runs/39843484211)
(`pages build and deployment`, sha `79fd927`) completed successfully at
2026-10-22 19:55 UTC. Checked just after that: tokens 4967–5026 return HTTP 200
and a JSON body. `media/flower_stories/6722.jpg` (token 4967) and `6781.jpg`
(token 5026) return HTTP 200 `image/jpeg`. Older paths still return HTTP 200
(`.../4966`, `.../1`, `meta/base/flower_stories_vol3/675`,
`meta/polygon/ai_play/675`).

The run uploaded an artifact of 3,142,791,886 bytes and logged:

> Uploaded artifact size exceeds the allowed size of 1 GB. Deployment might fail.

This run still published. The git repo size field (~2.4 GB) is the packed
history. The published site is the working tree: `meta/` ~161 MB and `media/`
~3.0 GB.

## Why the site was not switched to a smaller Actions artifact

A workflow that deploys only `meta/`, `media/`, and `.nojekyll` is the same
~3.1 GB the legacy builder already uploads. It does not fit under 1 GB.

`media/` breakdown: flower_stories 859 MB, nature_stories 754 MB, xrpl 650 MB,
nature-jam-2 425 MB, sui 289 MB. Avalanche, Base, and Polygon metadata, plus
the Solana mirrors, point `image` at
`https://jackbeatnic.github.io/jb-nft-assets/media/...`. Omitting one of those
folders would 404 images OpenSea already loads. XRPL metadata mostly uses
jsDelivr already; dropping `media/xrpl` would still leave the site far above
1 GB.

Replacing the legacy publisher with that workflow would not make new token ids
appear faster, and a hard 1 GB reject would stop future publishes. The last
green deployment would stay up, which is safe, but it would not serve newer
ids. A media-host split (image URLs moved in the same snapshot) needs a
separate decision. It is not a baseURI change, and it is not done here.

## Verify

```bash
curl -sL -o /dev/null -w "%{http_code}\n" https://jackbeatnic.github.io/jb-nft-assets/meta/avalanche/flower_stories/4996
curl -sL -o /dev/null -w "%{http_code}\n" https://jackbeatnic.github.io/jb-nft-assets/meta/avalanche/flower_stories/5026
curl -sL https://jackbeatnic.github.io/jb-nft-assets/meta/avalanche/flower_stories/4967
curl -sL -o /dev/null -w "%{http_code}\n" https://jackbeatnic.github.io/jb-nft-assets/media/flower_stories/6722.jpg
```

Expect `200`, and a JSON body whose `image` URL also returns `200`.

## Next mint batch

1. Push metadata and the matching `.jpg` files to `main` together.
2. Wait until `pages build and deployment` for that commit is green before
   refreshing OpenSea. At the current size that takes about 5 minutes. A
   refresh during the run still reads the previous snapshot (HTTP 200 for old
   ids, HTTP 404 for the new ones).
3. A cancelled run means a newer push replaced it. Wait for the newest run.
4. If a run fails on size or the 10 minute limit, do not delete the last green
   deployment and do not publish a partial tree. Plan a media host split
   first, then change image URLs and the Pages artifact in one snapshot.
