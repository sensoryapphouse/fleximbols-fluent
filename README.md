# PiCom Fluent Symbols — packed assets

**© Sensory App House Ltd — licensed [CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/)** (see [LICENSE](LICENSE)).

This repository holds the **PiCom Fluent** AAC symbol set (55,221 SVGs) packed for
efficient delivery to the [Fleximbols browser](https://fleximbols.pages.dev) over a
CDN, without hosting 55k individual files.

## Format

- **`pack-000.bin` … `pack-015.bin`** — 16 shards, each < 18 MB. Every SVG is stored as
  a **raw-DEFLATE** blob; the shards are those blobs concatenated.
- **`index.json`** — the directory: `{ "files": { "<relative/path.svg>": [shard, byteOffset, compressedLen, rawLen] } }`.

## How a client reads one symbol

1. Fetch `index.json` once.
2. Look up the entry → `HTTP Range: bytes=<offset>-<offset+compressedLen-1>` on `pack-<shard>.bin`
   (jsDelivr serves `206 Partial Content`).
3. Inflate the returned bytes with `DecompressionStream('deflate-raw')`.

So each symbol costs only its own few KB — the whole set is never downloaded.

Served via jsDelivr: `https://cdn.jsdelivr.net/gh/sensoryapphouse/fleximbols-fluent@main/…`
