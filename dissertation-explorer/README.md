# Murder by Inches — Dissertation Explorer

An interactive timeline of the 308 dated actions, figures, and events documented in the dissertation *Murder by Inches*. Explore the material through the dissertation's own structure (its four "spaces of protest," plus the introduction and conclusion), through five cross-cutting themes, or through the 48 recurring named figures who tie the narrative together.

## View it live

The easiest way to use this is via **GitHub Pages**:

1. Push this repo (or this folder, if it's part of a larger repo) to GitHub.
2. Go to **Settings → Pages** in the repo.
3. Under "Build and deployment," set **Source** to "Deploy from a branch," pick your default branch (e.g. `main`) and the `/ (root)` folder — or `/docs` if you place this folder there instead.
4. Save. GitHub will publish the site at `https://<your-username>.github.io/<repo-name>/` within a minute or two.

Once live, open that URL — no build step, no dependencies to install.

## Running it locally

This page loads its data from the `data/` folder at runtime via `fetch()`, so it needs to be served over HTTP — opening `index.html` directly from disk (a `file://` URL) will fail in most browsers because they block local `fetch` calls for security. From this folder, run:

```bash
python3 -m http.server 8000
```

then visit `http://localhost:8000/` in a browser. (Any static file server works — `npx serve`, VS Code's Live Server extension, etc.)

## What's inside

```
.
├── index.html          # page structure, styles, and all interactive logic
├── data/
│   ├── events.json      # 308 timeline events
│   ├── figures.json     # 48 recurring named figures + co-occurrence links
│   └── chapters.json    # labels/colors for the 7 chapter categories
└── README.md
```

Keeping the data in separate JSON files (rather than inlined in the HTML) keeps `index.html` readable and makes the underlying dataset easy to diff, review, or reuse on its own — useful if this repo is reviewed on GitHub rather than just run.

### Data fields

Each entry in `data/events.json`:

| field | meaning |
|---|---|
| `id` | stable identifier, e.g. `e147` |
| `year` / `endYear` | numeric year (`endYear` is `null` for single-date events) |
| `displayDate` | human-readable date string shown in the UI |
| `headline` | short title for the event |
| `text` | fuller description, shown in the detail panel |
| `chapter` | which of the 7 dissertation sections the event comes from |
| `theme` | one of 5 cross-cutting analytical themes |
| `figures` | names of recurring figures mentioned in this event |

Each entry in `data/figures.json`:

| field | meaning |
|---|---|
| `id` | stable identifier, e.g. `fig-solomon-attaquin` |
| `name` | canonical display name |
| `count` | number of events this figure appears in |
| `events` | ids of those events |
| `coOccurs` | other recurring figures who appear in events with this one, with a count |

`data/chapters.json` maps each of the 7 `chapter` values used in `events.json` to a display label, short label, and legend color.

## Features

- **Timeline** — a zoomable, pannable D3 view of all 308 events, positioned by year with automatic lane-stacking so densely-packed years (peaking at 14 events in both 1833 and 1869) don't overlap.
- **Chapter legend** — click a chip to show/hide events from that section of the dissertation.
- **Theme filter** — a secondary, cross-cutting set of five analytical categories; selecting one dims everything else.
- **Notable Figures panel** — all 48 recurring people, sorted by frequency. Click one to trace a connecting line through every event they appear in and see who they most often appear alongside; click a co-occurring name to jump to them.
- **Search** — free-text filter across headlines and event descriptions.
- **Detail panel** — click any dot for the full text of that event and the figures involved.

## Provenance

The underlying dataset was extracted from the dissertation's full text (front-matter timeline, chapters, and conclusion), deduplicated, and cross-referenced against the dissertation's own chapter structure to assign each event's primary category. Recurring figures were identified via a curated name/alias list and cross-referenced across all 308 entries to compute co-occurrence.
