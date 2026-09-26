# Tarasiuk Lab Matura Status

Public static status board for the Warsaw Model Trainers workstream.

## Ownership

- Matura Hack / Grok Bot owns refreshing this public board, including the three prize tabs and the five public CKE columns.
- Claude must not refresh this board.
- Claude adversarially reviews every commit here and in OrestTa/matura-model-trainers-hackathon.

## Refresh

1. Update `public_status.json` if you generate it; otherwise update `status.json`.
2. Update `scores.json` with the latest public CKE category splits, or mirror the same data under `status.json -> cke -> runs`.
3. Commit and push to `main`.
4. GitHub Pages republishes from the repository root.

## CKE category schema

The board now reserves five public CKE columns for every base run and every improvement stage:

- `text_open`
- `text_closed`
- `image_open`
- `image_closed`
- `essay`

Missing values must stay `null` and render as `—` on the site. Do not invent numbers.

The page loads data in this order:

1. `public_status.json` if present
2. `status.json`
3. optional `scores.json` overlays or supplements the CKE stage breakdowns

When full per-item CKE results are available, prefer supplying them and letting the board recompute the five public columns from:

- `gold_category`
- `needs_image`
- per-item score/correctness fields

Legacy `by_category` summaries (`closed_choice`, `matching`, `short_open`, `source_analysis`, `true_false`, `essay`) are not enough to reconstruct the text/image split except for `essay`, so the board leaves the other columns as `null` / `—` unless per-item results are present.

### Expected JSON shape

```json
{
  "updated": "2026-09-26T14:33:36.207784+02:00",
  "stages": [
    {
      "name": "base",
      "model": "Qwen2.5 3B",
      "status": "COMPLETED",
      "text_open": null,
      "text_closed": null,
      "image_open": null,
      "image_closed": null,
      "essay": null,
      "overall_pct": 26.7
    }
  ]
}
```

If you have per-item result JSON, a stage can also point at it:

```json
{
  "stages": [
    {
      "name": "base",
      "results_path": "./results/qwen25-3b-base.json"
    }
  ]
}
```

The board will try to recompute `text_open`, `text_closed`, `image_open`, `image_closed`, `essay`, and `overall_pct` from that result file when possible.

## Prize-track tabs

The page also supports a top-level `prize_tracks` array in `status.json` to drive the three board tabs. Example:

```json
{
  "prize_tracks": [
    {
      "id": "best-matura-score",
      "title": "Best matura score",
      "goal": "Highest honest Sunday exam score.",
      "status": "RUNNING",
      "summary": "Current leader and blockers.",
      "sections": [
        { "title": "Baselines (honest bare)", "stage_ids": ["cke-3b-base"] },
        { "title": "Interim results", "stage_ids": ["cke-3b-history-v2"] },
        { "title": "Best post-training / improvement stages", "stage_ids": ["cke-3b-modal"] }
      ]
    }
  ]
}
```

## History Ext Multi-Year

Multi-year corpus work tracks Formula 2023 May history papers for 2023–2026 in `data/history_extended/formulka-2023/`.
The first pack conversion target is `history-2024-05`, producing `exam.json`, cropped images, and `answers-template.json`.
`history-2023-mock-v1` remains the only submission-quality gauge; the wider corpus adds coverage but does not replace that benchmark.
Public status now also records a format-complete Forgehand official mock on `history-2023-mock-v1` for bare `Qwen2.5-7B-Instruct-AWQ`: 37/37 nonempty answers, essay regenerated to 811 words, and about 157s wall once running under the serial HF AutoAWQ path. This is status-only evidence and not an official grade.
Next up is `Bielik-4.5B FP8` via vLLM under the `<=8.0 GB` base cap; there is no public `Bielik-11B` lane in this board.
Formula 2015 stays deferred, and this conversion work does not need L40S capacity yet.
