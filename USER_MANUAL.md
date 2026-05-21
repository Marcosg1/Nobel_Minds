# Nobel Minds — User Manual

This document explains how to run, explore, and maintain the **Nobel Minds** dashboard.

---

## What this project is

**Nobel Minds** is a single-page, browser-based data visualization of Nobel laureates (1901–2014). The main deliverable is `index.html`: a linked dashboard with four D3 charts (timeline, world map, category bar chart, gender line chart) and shared filters in a left sidebar.

---

## Requirements

| Requirement | Notes |
|-------------|--------|
| **Modern web browser** | Chrome, Firefox, Safari |
| **Internet access** | Required on first load for D3 from CDN and world map GeoJSON from GitHub. |
| **VS Code + Live Server** (recommended) | [Live Server](https://marketplace.visualstudio.com/items?itemName=ritwickdey.LiveServer) extension serves the project over HTTP. This is how I ran the dashboard. |

There is **no** `npm install`, `package.json`, or build step for the dashboard itself.

---

## Quick start

### VS Code Live Server

This is the method used during development. Tested with the **Live Server** extension (Ritwick Dey) in VS Code.

1. **Get the project**  
   Clone or download the repository.

2. **Open the project folder in VS Code**  
   Use **File → Open Folder** and select the project root (the folder that contains `index.html` and `NobelData/`). Serving from the correct root matters so `NobelData/processed/PrizeWinners.csv` loads.

3. **Install Live Server** 
   In VS Code: **Extensions** (`Cmd+Shift+X` / `Ctrl+Shift+X`) → search **Live Server** → install **Live Server** by Ritwick Dey.

4. **Start the server**  
   - Right-click `index.html` in the Explorer → **Open with Live Server**, **or**  
   - Open `index.html` in the editor and click **Go Live** in the status bar (bottom right).

5. **Browser opens automatically**  
   Live Server usually opens something like:

   ```
   http://127.0.0.1:5500/index.html
   ```

   (Port may differ if 5500 is in use; check the status bar.)

6. **Confirm it loaded**
   - The page title is **Nobel Minds**.
   - The left sidebar shows **Filters** (Award years, Category, Sex, Country).
   - Four chart sections appear: timeline, map, bar chart, line chart.
   - Open the browser developer console (F12). You should see: `Loaded winners data:` followed by an array.

7. **Stop the server** when finished: click **Port: 5500** (or your port) in the status bar → **Stop Live Server**.

## Libraries and versions

Libraries are loaded from CDNs in HTML (not bundled). Versions below were verified from the CDN files on **May 2026**.

| Library | Version | Used in | CDN / source |
|---------|---------|---------|----------------|
| **D3.js** | **7.9.0** | `index.html` | `https://d3js.org/d3.v7.min.js` |
| **World GeoJSON** | commit on `master` branch | Map in `index.html` | `https://raw.githubusercontent.com/johan/world.geo.json/master/countries.geo.json` |

**Not used:** React, Vue, jQuery, npm packages, or a CSS framework. Styling is custom in `css/main.css`.

**Python (data prep only):** Python **3.x** with the standard library (`csv`, `pathlib`, `re`, `unicodedata`, `collections`). No third-party Python packages.

---

## Project layout

```
Nobel_Minds/
├── index.html              ← Main dashboard (open via HTTP server)
├── css/main.css            ← Styles
├── NobelData/
│   ├── raw/                ← Original Nobel Database CSVs (source)
│   │   ├── Award Categories.csv
│   │   ├── Countries.csv
│   │   ├── Prizes.csv
│   │   ├── Winners.csv
│   │   └── Years.csv
│   └── processed/
│       ├── PrizeWinners.csv ← Primary data file for the dashboard
│       ├── Prizes.csv
│       └── Winners.csv
```

---

## Using the dashboard

### Default view

On first load, filters are **not** “all years.” The default award-year range is **1901–1916** so the timeline and charts start with a focused window. To see the full dataset, either:

- Drag the **Award years** brush in the sidebar to cover 1901–2014, or  
- Click **Reset filters** (restores 1901–1916 and clears category/sex/country selections), then widen the year range as needed.

### Global filters (left sidebar)

All charts share one filter state. Changing any control updates every chart.

| Control | How to use |
|---------|------------|
| **Award years** | Drag the mini brush under “Award years” to set min/max year. A readout shows the active range (e.g. `1901–1916`). Dragging to the full width clears the year cap (all years in data). |
| **Category** | Click **All** or individual category bubbles (Physics, Chemistry, etc.). Multiple categories can be selected; click again to deselect. |
| **Sex** | Same pattern as Category (**All**, **Male**, **Female**). |
| **Country** | Dropdown; **All** shows every country. |
| **Reset filters** | Restores default years (1901–1916), **All** for category and sex, country **All**, and closes any open detail panels. |

### Timeline chart

| Action | Effect |
|--------|--------|
| **Hover** a laureate dot | Tooltip with name, year, category, country. |
| **Click** a dot | Opens the official Nobel Prize page in a new tab (when a link exists in the data). |
| **Drag** the brush on the **lower** strip | Zooms the year range; syncs with the sidebar **Award years** control. |
| **Hover** the WWII band (1940–1942) | Tooltip explaining reduced prizes during that period. |

### World map

| Action | Effect |
|--------|--------|
| **Hover** a shaded country | Tooltip with country name and prize count (for current filters). |
| **Click** a country | Side panel with laureate list and counts; links to Nobel site when available. |
| **Close** | **Close** button on the panel. |

Selecting a country on the map also sets the **Country** filter to that nation (linked behavior).

### Bar chart (categories)

| Action | Effect |
|--------|--------|
| **Hover** a bar | Tooltip with category and count. |
| **Click** a bar | Detail panel with laureates in that category under current filters. |
| **Close** | **Close** on the panel. |

### Line chart (gender over time)

| Action | Effect |
|--------|--------|
| **Hover** a point | Tooltip with year and male/female counts. |
| **Click** a point | Detail panel for that year (and gender series clicked). |
| **Close** | **Close** on the panel. |

### Keyboard shortcuts

There are **no** keyboard shortcuts. The UI is mouse-driven (touch should work on buttons and brushes where the browser supports it).

---

## Data sources

- **Laureate data:** [Nobel Database on GitHub (ali-ce/datasets)](https://github.com/ali-ce/datasets/tree/master/Nobel-Database) — snapshot through **2014**.
- **Country boundaries:** [world.geo.json](https://github.com/johan/world.geo.json) — `countries.geo.json`.
- **Official reference:** [Nobel Prize FAQ](https://www.nobelprize.org/frequently-asked-questions/)


---

## Author

Marcos Gallegos — CS 360 Data Visualization — University of San Francisco — 2026
