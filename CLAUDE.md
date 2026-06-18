# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What This Is

A client-side web application for automated seating assignment. Users upload a CSV of orders and a JSON seating plan; the app assigns seats using a best-available algorithm with orphan prevention. All processing is in-browser — no backend, no build step.

Live at: https://hongyanhuang.github.io/seating-agent/

## Running the App

No build process. Open `index.html` directly in a browser, or serve via any static file server:

```bash
python3 -m http.server 8080
# then open http://localhost:8080
```

Use `example_orders.csv` + `example_seating_plan.json` as test data. Use `example_duplicate_seats.json` to test duplicate detection.

## Architecture

Two standalone HTML files with all JS/CSS inline:

- **`index.html`** — Main seating assignment tool
- **`transformer.html`** — Utility to convert XLSX/CSV seating data into the JSON format `index.html` expects

No npm, no bundler, no framework. Tailwind is loaded from CDN. PapaParse handles CSV parsing. SheetJS handles XLSX in the transformer.

### State model (index.html)

Global variables hold all application state:
- `csvData` — parsed CSV orders
- `seatingPlanData` — merged JSON seating plan (array of seat-group arrays)
- `uploadedFiles` — array of `{name, data}` for each uploaded JSON file
- `quantityColumnName` — which CSV column holds ticket quantity
- `remainingSeats` — unassigned seats after processing

### Seating algorithm (`processAssignments`, ~line 656)

1. Sort orders descending by quantity
2. For each order, scan seat groups row by row for `N` consecutive available seats
3. Before assigning, call `wouldLeaveOrphan()` — skip any group that would leave exactly 1 seat remaining (orphan prevention)
4. Mark assigned seats unavailable; append seat columns to the order row

### JSON seating plan format

Array of arrays. Each inner array is a consecutive group of seats. Seat names follow `ZONE-SECTION--ROW---SEAT`:

```json
[["A-1--2---1", "A-1--2---2", "A-1--2---3"], ["A-1--3---1"]]
```

### CSV output columns added

`raw_data` (JSON array of all assigned seats), then `zone1, section1, row1, seat1` … `zoneN, sectionN, rowN, seatN` for each individual seat.

## Key Functions

| Function | File | Purpose |
|---|---|---|
| `processAssignments()` | index.html ~656 | Main orchestration: sort, assign, download |
| `wouldLeaveOrphan()` | index.html | Check if assigning N seats leaves exactly 1 remaining |
| `handleCSVFile()` | index.html ~210 | PapaParse CSV → `csvData` + table preview |
| `validateAndPreviewJSON()` | index.html ~534 | Merge uploaded files, detect duplicates, preview |
| `downloadResults()` | index.html | Build and trigger CSV download |

## Deployment

Push to `main` branch — GitHub Pages serves the repo root automatically.
