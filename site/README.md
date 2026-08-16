# site/ — resized images for the public gallery

These files are **derivatives**, not mint originals.

| File | Size | Use |
|------|------|-----|
| `{token_id}.thumb.webp` | 440×440 | grid thumbnail |
| `{token_id}.view.webp` | 1200×1200 | lightbox View |

Source: `backup_offline/by_collection/{collection_id}/media/{token_id}.jpg` (2048×2048 originals stay offline).

Do **not** put 2048 JPGs here. Rebuild:

```text
python3 raportowanie/buduj_site_media.py avalanche_nature_stories
```

Live:

```text
https://jackbeatnic.github.io/jb-nft-assets/site/avalanche_nature_stories/2.thumb.webp
```
