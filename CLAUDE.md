# CLAUDE.md — apache-spark-reels

Guidance for Claude Code when working in this repo. This is the **content** for
the **Apache Spark** topic in [graphl-mobile](../graphl-mobile), the technical
reels PWA. The app fetches this content at runtime via raw GitHub; it is **not**
bundled into the app. Repo: `github.com/schemabotview/apache-spark-reels`.

## Structure

```
scenes/   # currently EMPTY — SceneSpec data lives in graphl-mobile/src/data/scenes/<stem>.ts
tts/      # <stem>.tts — plain spoken prose (no markdown/code) for ChatterboxTTS
audio/    # <stem>.wav — generated narration, committed and served via raw GitHub
```

One reel = one `<stem>` shared across folders (e.g. `spark-rdd-api.tts`,
`spark-rdd-api.wav`). A scene's `audio` field defaults to its `id`.

## Pipeline (who does what)

1. **Scene** — author `graphl-mobile/src/data/scenes/<stem>.ts` and register it in
   `src/data/scenes/index.ts` (feed order = teaching order). `npm run build` is
   the only gate (runs DEV `validateLayout`).
2. **TTS** — write `tts/<stem>.tts` here: ~650–780 words (≈5 min), plain spoken
   prose, no markdown/code. Blank lines become ~300 ms pauses that pace the node
   reveals.
3. **Audio** — **the user** generates + pushes the `.wav` (Claude does not run
   the conda/audio/push step):
   ```bash
   # from ~/Apps/graphl-mobile:
   conda run -n chatterbox python scripts/generate_audio.py --topic apache-spark
   ```
   Then commit + push the `.wav` here so it serves via raw GitHub.

## Series conventions

This is an "API cheat-sheet" series decomposed from one dense master diagram into
focused per-topic reels. Key conventions (keep consistent):

- **Authoring grammar:** each scene = titled boxes built with the shared
  `section(meta, bands, opts)` helper in `src/data/layout/patterns.ts`; compose
  with `stack` (vertical bands) and `group(columns([...]))` for 2-up/3-up rows.
  Big boxes go 2-up/3-up to use width and limit grid rows (fixed canvas → more
  rows = smaller chips). Proportional/time-axis diagrams (memory model,
  windowing, watermarking) are hand-authored with explicit `cell` spans.
- **Color semantics (locked):** blue=read/input/pandas, red=RDD,
  orange=DataFrame/mode, green=SQL/write, purple=cross-cutting, teal=sub-box
  variety. Defined in `src/data/colors.ts`.
- **One reel per API** (DataFrame/SQL not split) — accept density, lean on
  `tight` + 2-up.

## Scene index

Legend: ✅ scene + tts done · 🔊 audio generated · 🔲 not started. Order = feed /
teaching order.

| #  | Stem                        | Title                  | Scene | TTS | Audio |
|----|-----------------------------|------------------------|-------|-----|-------|
| 1  | `spark-execution`           | Spark Execution Model  | ✅    | ✅  | 🔊    |
| 2  | `apache-spark-api-stack`    | API Stack (overview)   | ✅    | ✅  | 🔊    |
| 3  | `spark-rdd-api`             | RDD API                | ✅    | ✅  | 🔊    |
| 4  | `spark-dataframe-api`       | DataFrame API          | ✅    | ✅  | 🔊    |
| 5  | `spark-sql-api`             | SQL API                | ✅    | ✅  | 🔊    |
| 6  | `spark-pandas-api`          | Pandas API on Spark    | ✅    | ✅  | 🔊    |
| 7  | `spark-reading-sources`     | Reading Sources        | ✅    | ✅  | 🔊    |
| 8  | `spark-writing-sinks`       | Writing Sinks          | ✅    | ✅  | 🔊    |
| 9  | `spark-cache-persist`       | Cache & Persist        | ✅    | ✅  | 🔊    |
| 10 | `spark-performance-tuning`  | Performance Tuning     | ✅    | ✅  | 🔊    |
| 11 | `spark-structured-streaming`| Structured Streaming   | ✅    | ✅  | 🔊    |
| 12 | `spark-windowing`           | Windowing (time-axis)  | ✅    | ✅  | 🔲    |
| 13 | `spark-watermarking`        | Watermarking (time-axis)| ✅   | ✅  | 🔲    |

Streaming sub-series order: structured-streaming → windowing → watermarking.
Windowing & watermarking are time-axis diagrams (shared 25-col minute grid,
`edges: []`) — see `graphl-mobile/src/data/scenes/` for the worked cells.
