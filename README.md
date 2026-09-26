# Tarasiuk Lab Matura Status

Public static status board for the Warsaw Model Trainers workstream.

## Refresh

1. Update `public_status.json` if you generate it; otherwise update `status.json`.
2. Update `scores.json` with the latest public CKE category splits, or mirror the same data under `status.json -> cke -> runs`.
3. Commit and push to `main`.
4. GitHub Pages republishes from the repository root.

## CKE category schema

The board now reserves five public CKE categories for every base run and every improvement stage:

- `text_open`
- `text_closed`
- `image_open`
- `image_closed`
- `essay`

Missing values must stay `null` and render as `—` on the site. Do not invent numbers.

The page loads data in this order:

1. `public_status.json` if present
2. `status.json`
3. optional `scores.json` overlays or supplements the CKE run breakdowns

### Expected JSON shape

```json
{
  "updated": "2026-09-26T14:33:36.207784+02:00",
  "cke": {
    "categories": [
      { "id": "text_open", "label": "text open" },
      { "id": "text_closed", "label": "text closed" },
      { "id": "image_open", "label": "image open" },
      { "id": "image_closed", "label": "image closed" },
      { "id": "essay", "label": "essay" }
    ],
    "runs": [
      {
        "id": "cke-3b-base",
        "label": "3B base",
        "model": "Qwen2.5 3B",
        "stage": "base",
        "status": "COMPLETED",
        "overall_pct": 26.7,
        "categories": {
          "text_open": { "pct": 30.0, "n": "3/10" },
          "text_closed": null,
          "image_open": null,
          "image_closed": null,
          "essay": null
        }
      }
    ]
  }
}
```

`scores.json` can expose the same run records under either `cke_runs` or `cke.runs`; the page merges those rows by `id`.
