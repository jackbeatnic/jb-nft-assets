# jb-nft-assets — porządek plików (NS / FS / NJ × chainy)

To repo **nie jest galerią**. Tylko pliki pod URI NFT (meta JSON + media).  
Galeria (`jackbeatnic.github.io` → później Cloudflare) żyje osobno.

Pages root: `https://jackbeatnic.github.io/jb-nft-assets/`

---

## Zasada główna

| Co | Gdzie | Klucz |
|----|--------|--------|
| **1 JPG na dzieło** | `media/{series}/{art_id}.jpg` | `art_id` = numer dzieła w serii (nie „chain”) |
| **1 JSON na token w kontrakcie** | `meta/{chain}/{collection}/{token_id}` | `token_id` on-chain w **tej** kolekcji |
| JSON wskazuje JPG | pole `"image"` | ten sam URL media dla 2–3 chainów |

**Trojaczki (ten sam art na AVAX + Base + Polygon):**

```text
media/nature_stories/6000.jpg          ← JEDEN plik

meta/avalanche/nature_stories/6000     ← JSON, image → …/media/nature_stories/6000.jpg
meta/base/nature_stories/6000          ← ten sam image URL
meta/polygon/nature_stories/6000       ← ten sam image URL
```

Numery w meta mogą być te same „#6000” biznesowo, ale **ścieżka meta zawsze idzie przez chain + collection**.

---

## Katalogi

```text
jb-nft-assets/
  README.md
  STRUCTURE.md                 ← ten plik
  .nojekyll

  media/
    {series}/
      {art_id}.jpg             # art_id bez wiodących zer, np. 6000.jpg
      # opcjonalnie: {art_id}.webp

  meta/
    {chain}/
      {collection}/
        {token_id}             # JSON BEZ rozszerzenia .json (ERC-1155 {id})
```

### Dozwolone `series` (media — wspólne dla chainów)

| series | Znaczenie |
|--------|-----------|
| `nature_jam` | Nature Jam (wszystkie vol / chain jeśli ten sam art) |
| `nature_stories` | Nature Stories |
| `flower_stories` | Flower Stories |
| `based_ai` | Based AI (gdy potrzeba) |
| `other` | wyjątki |

### Dozwolone `chain` (meta)

| chain | Sieć |
|-------|------|
| `avalanche` | Avalanche C-Chain |
| `base` | Base |
| `polygon` | Polygon |
| `ethereum` | gdy kiedyś |
| `sui` | gdy meta off-chain pod docs (Sui zwykle inny pipeline) |

### Dozwolone `collection` (meta — folder = kontrakt / slug logiczny)

Używaj **snake_case**, stabilnych id jak w `kolekcje.json`:

| collection | Przykład |
|------------|----------|
| `nature_jam_vol2` | NJ vol.2 Avalanche |
| `nature_jam` | NJ vol.1 |
| `nature_stories` | NS MAIN na danym chain |
| `flower_stories` | FS MAIN |
| `nature_stories_vol3` | gdy osobny kontrakt vol |

**Nie mieszaj** plików z dwóch kontraktów w jednym folderze `collection`.

---

## URL-e (GitHub Pages)

```text
Meta:
https://jackbeatnic.github.io/jb-nft-assets/meta/{chain}/{collection}/{token_id}

Media:
https://jackbeatnic.github.io/jb-nft-assets/media/{series}/{art_id}.jpg
```

**On-chain baseURI** (jeden na kontrakt):

```text
https://jackbeatnic.github.io/jb-nft-assets/meta/{chain}/{collection}/{id}
```

(SeaDrop / OpenSea: placeholder `{id}` w stringu.)

---

## Partie mintów (przykład Twojego workflow)

1. **Avalanche** — partia 100 szt. NS #5901–#6000  
   - `media/nature_stories/5901.jpg` … `6000.jpg`  
   - `meta/avalanche/nature_stories/5901` … `6000`  
   - mint na kontrakcie AVAX  

2. **Base** — kontynuacja numeracji biznesowej, ale **nowe token_id** na kontrakcie Base (albo te same numery jeśli tak trzymacie)  
   - **NIE** kopiować JPG ponownie: w JSON Base tylko  
     `"image": ".../media/nature_stories/5950.jpg"`  
   - `meta/base/nature_stories/{token_id}`  
   - mint na Base  

3. **Polygon** — j.w., trzeci JSON, ten sam `image`.

Dzięki temu 50 nowych mintów na Base ≠ 50 nowych megabajtów, jeśli art już był na AVAX.

---

## LEGACY — Nature Jam vol.2 (nie ruszać ścieżek)

Już na łańcuchu i w Pages — **zamrożone URL-e**:

```text
meta/avalanche/nature-jam-2/{token_id}     # uwaga: myślnik w nazwie folderu
media/nature-jam-2/{token_id}.jpg
baseURI: .../meta/avalanche/nature-jam-2/{id}
```

- `nature-jam-2` = historyczna nazwa folderu (slug OS).  
- Nowe kolekcje: **`nature_jam_vol2` snake_case** tylko jeśli **nowy** kontrakt / nowy baseURI.  
- **Nie przenosić** plików NJ vol.2 do `nature_jam` bez migracji baseURI (zostawiłoby 404).

Mapowanie art: dla NJ2 `art_id` ≈ `token_id` (1 chain). Źródło offline: `SUI/#{token_id - 41}`.

Szczegóły składu: w monorepo `mintowanie/NJ_VOL2_INVENTORY.md`.

---

## JSON — minimalny schemat

```json
{
  "name": "JB NJ #1056",
  "description": "…\nAvalanche Edition | Jack Beatnic 2026",
  "external_url": "https://jackbeatnic.github.io",
  "image": "https://jackbeatnic.github.io/jb-nft-assets/media/nature-jam-2/270.jpg"
}
```

Opcjonalnie później: `attributes`, `animation_url`.  
Przy migracji Arweave: ten sam układ folderów; zmieniasz tylko host w `image` + baseURI.

---

## Czego unikać

- JPG w `meta/`  
- Meta z trzech chainów w jednym katalogu bez `{chain}/`  
- Duplikat `6000.jpg` w `media/avalanche/` i `media/base/`  
- Wr//rzucanie assetów do repo **galerii**  
- Zmienianie legacy `nature-jam-2` „dla estetyki” bez planu baseURI  

---

## Checklista nowej partii

1. Ustal `series`, `art_id`, `chain`, `collection`, zakres `token_id`.  
2. Wrzuć brakujące **media** (tylko nowe art_id).  
3. Wygeneruj **meta** dla tego chain/collection.  
4. `git push` → poczekaj na Pages.  
5. Mint on-chain (`mint_token` / batch) z baseURI wskazującym ten `meta/{chain}/{collection}/{id}`.  
6. Zaktualizuj inventory serii w monorepo.  
