# SolidCore Studio Hub feed

The files the SolidCore hubs read. Apps fetch them from
`https://raw.githubusercontent.com/ASGrincewicz/SolidCoreStudioHubFeed/main/`, so a change goes live
(within a few minutes, GitHub's cache) as soon as it's pushed. No app release is needed.

This repository is published from the `feed/` folder of the SolidCore repository with
`scripts/publish-feed.sh`. Edit the files there, not here.

## tools/: one update feed per tool

Each SolidCore tool has its own file, so people only hear about the tools they use:
`tools/world-map-editor.json`, `tools/studio.json`, and later `tools/material-editor.json` and so on.
When a tool's hub opens it reads its own file and, if `version` is newer than the running app, shows
"Version x is out" with the summary; clicking opens `link` in the browser. Nothing is downloaded or
installed by the app.

```json
{
  "format": 1,
  "tool": "world-map-editor",
  "name": "SolidCore World Map Editor",
  "version": "0.11.0",
  "date": "2026-10-20",
  "summary": "One or two sentences on what's new, shown in the banner.",
  "link": "https://solid24systems.com/world-map-editor",
  "demoLink": "https://solid24systems.com/world-map-editor/demo",
  "history": [ { "version": "0.11.0", "date": "2026-10-20", "summary": "…" } ]
}
```

- `version` (major.minor.patch) and `link` (`https://` only) are required; a file without them is ignored.
- `demoLink` is optional: where the demo and preview builds are sent. Without it they get `link`.
- `history` is the list of releases, newest first. Apps don't show it yet; it's kept for a "what's new" list.
- Unknown fields are ignored, so the format can grow.
- Bump `version` only once the new build can be downloaded from `link`. The publish script refuses a
  version ahead of the one in SolidCore's project file.

## community/: the Community tab

Worlds people shared, sent in by email from the editor (File → Share with the Community…). Each has its
own folder, `community/<id>/`, with the world (`<id>.solidworld`) and its picture (`thumbnail.png`), and
`community/index.json` lists them, newest first. The hub shows the newest 10; opening one downloads its
world and opens a copy.

Add one with `scripts/add-community-world.sh`, which checks the world, names the files, makes the
picture and puts it at the top of the list:

```json
{
  "format": 1,
  "items": [
    {
      "id": "sunken-keep",
      "title": "Sunken Keep",
      "summary": "A flooded castle with a secret under the moat.",
      "genre": "Metroidvania",
      "date": "2026-10-08",
      "credits": ["Alex Kim", "Sam Rivera"],
      "world": "community/sunken-keep/sunken-keep.solidworld",
      "thumbnail": "community/sunken-keep/thumbnail.png"
    }
  ]
}
```

- `id`, `title` and `world` are required. `world` and `thumbnail` must be inside the item's own folder
  (`community/<id>/…`, lowercase letters, digits, dots, dashes and underscores); anything else is ignored.
- A world must carry its credits (who made it) or the app refuses to open it.
- Worlds can be up to 8 MB and pictures up to 2 MB (PNG or JPEG; the card shows it 250×140, cropped).
