# SolidCore Studio Hub feed

The files the SolidCore Studio and World Map Editor hubs read. Apps fetch them from
`https://raw.githubusercontent.com/ASGrincewicz/SolidCoreStudioHubFeed/main/`, so a change goes live
(within a few minutes, GitHub's cache) as soon as it's pushed. No app release is needed.

This repository is published from the `feed/` folder of the SolidCore repository with
`scripts/publish-feed.sh`. Edit the files there, not here.

## updates.json: "a new version is out"

When the hub opens it reads its product's entry and, if the version is newer than the running app,
shows a banner that opens `link` in the browser. Nothing is downloaded or installed by the app.

```json
{
  "format": 1,
  "products": {
    "world-map-editor": {
      "version": "0.11.0",
      "date": "2026-10-20",
      "summary": "One or two sentences on what's new, shown under the banner.",
      "link": "https://solid24systems.com/world-map-editor",
      "demoLink": "https://solid24systems.com/world-map-editor/demo"
    },
    "studio": { "version": "0.7.0", "date": "2026-10-20", "summary": "…", "link": "https://…" }
  }
}
```

- Products: `world-map-editor` and `studio`. The demo and preview builds read the same entry as
  their full product.
- `version` (major.minor.patch) and `link` (`https://` only) are required; an entry without them is ignored.
- `demoLink` is optional: where the demo and preview builds are sent. Without it they get `link`.
- `date` and `summary` are optional. Unknown fields are ignored, so the format can grow.
- Bump `version` only once the new build can be downloaded from `link`.

## learn.json: the Learn tab

Cards on the hub's Learn tab. The format is in SolidCore's docs/STUDIO_DESKTOP_PLAN.md ("Learn feed").
In short: `title` and `link` (`https://…` or `studio:sample`) are required; `summary`, `kind`
(`video`, `docs`, `article`, `sample`), `thumbnail` (`https://…` PNG or JPEG, 2 MB at most), `date`
(yyyy-MM-dd) and `section` are optional.
