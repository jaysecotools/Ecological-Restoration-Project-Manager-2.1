Ecological Restoration Project Manager (ERPM)

A comprehensive, browser-based application for planning, tracking, and managing ecological restoration projects. Built as a single-page application with offline-first data storage, interactive mapping, and reporting capabilities.

---

Table of Contents

1. Overview
2. Features
3. Getting Started
4. User Guide
   · Dashboard
   · Projects
   · Monitoring
   · Reports
   · Team
   · Data Management
5. Technical Architecture
6. Data Model
7. Storage & Backup
8. Customization
9. Browser Support
10. Troubleshooting
11. Limitations & Roadmap

---

Overview

ERPM is a client-side web application designed for ecologists, restoration project managers, field technicians, and researchers who need to:

· Catalogue restoration projects (riparian, wetland, forest, coastal, grassland, urban, mine site, agricultural)
· Track project progress, budgets, and milestones
· Log monitoring points and field observations with geo-referenced data
· Attach field photos to projects, monitoring points, and observations
· Generate reports (PDF / HTML / CSV)
· Manage team members and their project assignments
· Export/import full datasets as JSON for backup or sharing

The entire application runs in the browser — no server, no installation, no internet required after the initial page load (except for map tiles).

---

Features

🗺️ Interactive Mapping (Leaflet)

· Overview map showing all project locations, colour-coded by status
· Click-to-select location picker when creating projects and monitoring points
· Project detail map showing monitoring points within the project area
· Automatic zoom/pan to markers, fallback to an Australian overview when no projects exist

📊 Dashboard Analytics

· Live counters: active projects, monitoring points, observations, pending/overdue actions, team members
· Chart.js visualisations: project status (doughnut), restoration types (bar), project timeline (line), monitoring frequency (bar)
· Recent activity feed (last 100 actions logged)

📁 Project Management

· Full CRUD on projects
· Filter by status (active/planned/completed/on-hold) and type
· Search by name, location, or description
· Milestones with completion tracking and progress bar
· Photo gallery per project

🔬 Monitoring & Observations

· Multiple monitoring point types: vegetation, water quality, wildlife, soil, erosion
· Observations with date, notes, weather, temperature, and photos
· Filter observations by project and date range (today / week / month)

👥 Team Management

· Roles: project manager, ecologist, field technician, volunteer, researcher, administrator
· Assign members to multiple projects
· Filter by role, search by name/email

📄 Reporting

· Report types: Summary, Progress, Monitoring Data, Full Project Report
· Formats: PDF (jsPDF), HTML, CSV
· Optional date-range filtering
· Optional per-project scoping

💾 Data Management

· All data persisted to browser localStorage
· Export full dataset as versioned JSON
· Import from JSON with confirmation prompt
· Manual backups (last 5 retained automatically)
· Storage usage statistics

---

Getting Started

Prerequisites

· A modern web browser (Chrome, Firefox, Edge, or Safari — recent versions)
· Internet connection for the first load (CDN dependencies + map tiles)

Installation

1. Save the application HTML file (e.g. erpm.html) anywhere on your computer.
2. Open it in your browser by double-clicking the file, or drag-and-drop into a browser window.
3. That's it — the app boots with sample data on first run.

First-Run Experience

On first launch, the app seeds:

· 1 sample project ("Riverside Wetland Restoration") — a 5.2 ha wetland in NSW
· 1 monitoring point ("Wetland Vegetation Plot A")
· 1 team member ("John Smith", Project Manager)

You can freely delete these and start fresh.

Recommended: Host Locally

For better security and consistent localStorage persistence, serve the file from a local web server rather than opening via file://:

```bash
# Python 3
python -m http.server 8000

# Node.js (with npx)
npx serve .

# PHP
php -S localhost:8000
```

Then navigate to http://localhost:8000/erpm.html.

⚠️ Note: Browsers may isolate localStorage per-origin. If you open the file via file:// and later switch to http://localhost, you'll see different data stores.

---

User Guide

Dashboard

The landing page provides at-a-glance status of your entire program.

Widget Description
Active Projects Count of projects with status active (with total count as a subtitle)
Monitoring Points Total monitoring points (with total observations as a subtitle)
Pending Actions Uncompleted milestones (with overdue count as a subtitle)
Team Members Total team members (with active count as a subtitle)
Project Locations Interactive map with colour-coded pins
Recent Activities Last 5 actions (create/update/delete events)

Map pin colours:

· 🟢 Green — Active
· 🟠 Amber — Planned
· 🔵 Blue — Completed
· 🔴 Red — On Hold

Projects

Navigate to Projects in the top navigation bar.

Creating a Project

1. Click ➕ New Project
2. Fill in the form:
   · Project Name (required)
   · Restoration Type (required) — Riparian, Coastal, Wetland, Forest, Grassland, Urban, Mine Site, Agricultural
   · Status — Planned, Active (default), Completed, On Hold
   · Location Name (required) — e.g. "Smith River, NSW"
   · Map Location (required) — click anywhere on the picker map to drop a pin
   · Start Date (required), End Date (optional)
   · Area (ha), Budget ($) (optional)
   · Description (optional)
   · Photos — click the upload area; each file must be ≤ 2 MB
3. Click Save Project

Filtering & Searching

· Search box — matches against name, location, and description
· Type dropdown — filter by restoration type
· Status tabs — All / Active / Planned / Completed

Project Actions

Each project card offers three buttons:

· 👁 View — opens a detail modal (overview, stats, photo gallery, timeline, embedded map)
· ✏️ Edit — loads the project into the form
· 🗑 Delete — removes the project and all its monitoring points (irreversible; confirm prompt shown)

Progress Calculation

Progress = completed milestones ÷ total milestones × 100. Projects without milestones display no progress bar.

ℹ️ Milestones can only be edited by manipulating the underlying JSON (see Data Model and Export/Import). The UI does not yet expose milestone CRUD.

Monitoring

Navigate to Monitoring in the top navigation bar. Three tabs are available.

All Points

Grid of monitoring point cards. Each card shows:

· Point name & type (with icon)
· Parent project
· First photo thumbnail
· Observation count & date of most recent observation
· Buttons: + Observation, View, Delete

Search by name; filter by project.

Add New

Form fields:

· Project (required) — dropdown of existing projects
· Point Name (required)
· Point Type (required) — Vegetation, Water Quality, Wildlife, Soil, Erosion
· Map Location (required) — click the picker map
· Photos (optional) — ≤ 2 MB per file

Observations

Flat list of observations from all points, sortable by date (newest first). Filter by:

· Project — any specific project or all
· Date — All / Today / This Week / This Month

Each observation shows parent point & project, date, notes, weather, temperature, and photo thumbnails (click to enlarge).

Adding an Observation

1. From the All Points tab, click ➕ Observation on a point card
2. Fill in:
   · Date (required) — defaults to today
   · Notes (required)
   · Weather Conditions, Temperature (°C) (optional)
   · Photos (optional)
3. Click Save Observation

Reports

Navigate to Reports in the top navigation bar.

Live Charts

Four auto-updating charts:

1. Project Status — doughnut breakdown by status
2. Restoration Types — bar chart of project counts per type
3. Project Timeline — line chart of new projects per month
4. Monitoring Frequency — bar chart of observations over the last 6 months

Generating a Report

1. Select Project — leave blank for all projects
2. Report Type — Summary, Progress, Monitoring Data, or Full
3. Output Format — PDF, HTML, CSV
4. Date Range — optional from → to filter
5. Click Generate Report

⚠️ Currently only the PDF format actually generates a downloadable file (via jsPDF). HTML and CSV options are placeholders that show a success toast — see Limitations.

Team

Navigate to Team in the top navigation bar.

Team members are shown in a table: Name, Role, Email, Status, Assigned Projects, Actions.

Adding a Team Member

1. Click ➕ Add Member
2. Fill in:
   · Full Name (required)
   · Role (required) — Project Manager, Ecologist, Field Technician, Volunteer, Researcher, Administrator
   · Email (required)
   · Phone (optional)
   · Status — Active or Inactive
   · Assigned Projects — multi-select (hold Ctrl / Cmd)
3. Click Save Member

Data Management

Navigate to Data in the top navigation bar.

Card Action
Export Data Downloads erpm-data-YYYY-MM-DD.json containing all projects, monitoring points, activities, and team members
Import Data File picker for .json; replaces all current data after confirmation
Backup Now Writes a snapshot into localStorage under erpm-backup-YYYY-MM-DD; retains only the 5 most recent backups
Data Statistics Counts of every record type

The Storage Information card shows total data size (in KB), date of most recent backup, and schema version.

Recommended Backup Workflow

1. After significant data entry, click Backup Now
2. Weekly (or before clearing browser data), click Export Data and store the JSON file externally
3. To restore: Import Data → select the exported JSON

---

Technical Architecture

Stack

Layer Technology
Markup HTML5 (single file)
Styling Vanilla CSS with CSS custom properties (Material-inspired palette)
Logic Vanilla JavaScript (ES6+), no framework, no build step
Mapping Leaflet 1.9.4 + OpenStreetMap tiles
Charts Chart.js + date-fns adapter
PDF jsPDF 2.5.1
Screenshots html2canvas (loaded, currently unused)
Icons Font Awesome 6.4.0
Fonts Google Fonts (Roboto)
Persistence localStorage

File Structure

The app is intentionally a single self-contained HTML file. Sections:

```
<header>              Navigation + logo
<main>
  ├─ #dashboard       Stats, project map, activity feed
  ├─ #projects        Project list + create/edit form
  ├─ #monitoring      Points list + new-point form + observations
  ├─ #reports         Charts + report generation
  ├─ #team            Team member table + form
  └─ #data            Export/import/backup
<div> modals          Project detail, add-observation
<div> toasts          Notification container
<script>              All logic (~1800 lines)
```

State Management

A single global state object holds all runtime data:

```js
const state = {
  projects: [],              // see Data Model
  monitoringPoints: [],
  activities: [],
  teamMembers: [],
  selectedTab: 'dashboard',
  // Leaflet map instances
  projectMap, monitoringMap, projectLocationMap,
  monitoringLocationMap, projectDetailMap,
  projectLocationMarker, monitoringLocationMarker,
  // Chart.js instances
  charts: {},
  // In-memory photo staging before save
  tempPhotos: { project: [], monitoring: [], observation: [] },
  // Modal context
  currentProject, currentMonitoringPoint, currentObservation,
  // Filters
  currentProjectFilter: 'all',
  searchFilters: { projects: '', monitoring: '', team: '' }
};
```

Mutating functions always call saveData() to persist to localStorage.

Key Functions Reference

Function Purpose
loadData() Reads localStorage; seeds sample data if empty
saveData() Writes all four collections back to localStorage
renderProjects() Rebuilds project grid from state.projects
renderMonitoringPoints(filter) Rebuilds monitoring grid
renderTeamMembers(roleFilter) Rebuilds team table
renderObservations() Rebuilds observation list
updateDashboard() Refreshes all dashboard counters + activity feed
updateCharts() Refreshes all Chart.js instances
updateProjectMapMarkers() Rebuilds Leaflet markers, auto-fits bounds
initMaps() One-time initialisation of all four Leaflet maps
initProjectDetailMap(project) Creates an on-demand detail map
showToast(msg, type) Displays a transient notification (success, error, warning, info)
addActivity(projectId, type, description) Appends to activity log (capped at 100)

Event Handling

All form submissions use a single delegated listener on document:

```js
document.addEventListener('submit', function(e) {
  if (e.target.matches('#project-form')) handleProjectSubmit(e);
  else if (e.target.matches('#monitoring-form')) handleMonitoringSubmit(e);
  // ...
});
```

Navigation uses per-link listeners attached during initNavigation().

---

Data Model

Project

```json
{
  "id": "project-1700000000000",
  "name": "Riverside Wetland Restoration",
  "type": "wetland",                 // riparian | coastal | wetland | forest | grassland | urban | mine | agricultural
  "status": "active",                // planned | active | completed | on-hold
  "location": {
    "name": "Smith River, NSW",
    "coords": [-33.8688, 151.2093]   // [lat, lng]
  },
  "description": "...",
  "startDate": "2023-01-01",
  "endDate": "2023-12-31",
  "area": 5.2,                       // hectares
  "budget": 125000,
  "photos": [
    {
      "id": "photo-...",
      "name": "site.jpg",
      "data": "data:image/jpeg;base64,...",  // stored inline
      "size": 123456,
      "type": "image/jpeg",
      "uploadedAt": "2023-01-01T00:00:00.000Z"
    }
  ],
  "milestones": [
    {
      "id": "1",
      "name": "Site Assessment",
      "date": "2023-01-15",
      "completed": true,
      "description": "Initial ecological assessment completed"
    }
  ]
}
```

Monitoring Point

```json
{
  "id": "monitoring-1700000000000",
  "projectId": "project-1700000000000",
  "name": "Wetland Vegetation Plot A",
  "type": "vegetation",              // vegetation | water-quality | wildlife | soil | erosion
  "coords": [-33.8690, 151.2095],
  "photos": [],
  "observations": [
    {
      "id": "obs-1700000000000",
      "date": "2023-05-01",
      "notes": "Observed regrowth of native sedges.",
      "weather": "Sunny",
      "temp": 22.5,
      "photos": []
    }
  ]
}
```

Team Member

```json
{
  "id": "member-1700000000000",
  "name": "John Smith",
  "role": "project-manager",         // project-manager | ecologist | field-technician | volunteer | researcher | administrator
  "email": "john.smith@example.com",
  "phone": "+61 412 345 678",
  "status": "active",                // active | inactive
  "projects": ["project-1700000000000"]
}
```

Activity

```json
{
  "id": "activity-1700000000000",
  "projectId": "project-1700000000000",
  "type": "project-creation",        // project-creation | project-update | project-deletion
  "date": "2023-01-01T12:00:00.000Z",
  "description": "Created new project: Riverside Wetland Restoration"
}
```

Exported Envelope

```json
{
  "version": 2,
  "exportedAt": "2023-06-01T12:00:00.000Z",
  "projects": [ ... ],
  "monitoringPoints": [ ... ],
  "activities": [ ... ],
  "teamMembers": [ ... ]
}
```

---

Storage & Backup

localStorage Keys

Key Content
erpm-projects JSON array of projects
erpm-monitoring JSON array of monitoring points
erpm-activities JSON array of activity log entries
erpm-team JSON array of team members
erpm-backup-YYYY-MM-DD Full backup snapshots (max 5 retained)

Quota Awareness

Browsers typically cap localStorage at 5–10 MB per origin. Because photos are stored as base64 inline, large photo collections will consume this quickly.

Best practice:

· Keep photos ≤ 2 MB (enforced) and prefer ≤ 300 KB
· Perform regular Export Data exports and prune older photos if space is tight
· Monitor the Storage Information card on the Data tab

Restore from Backup

Backups live in localStorage but there's no automatic restore UI. To restore manually, open your browser's DevTools console:

```js
// List backups
Object.keys(localStorage).filter(k => k.startsWith('erpm-backup-'));

// Restore a chosen backup
const backup = JSON.parse(localStorage.getItem('erpm-backup-2023-06-01'));
state.projects = backup.projects;
state.monitoringPoints = backup.monitoringPoints;
state.activities = backup.activities;
state.teamMembers = backup.teamMembers;
saveData();
location.reload();
```

For safety, prefer the Export → Import flow which is UI-supported.

---

Customization

Theming

All colours are declared as CSS custom properties at the top of the <style> block:

```css
:root {
  --primary: #2e7d32;         /* main green */
  --primary-light: #60ad5e;
  --primary-dark: #005005;
  --secondary: #ff8f00;       /* amber accent */
  --background: #f5f5f5;
  --paper: #ffffff;
  --text-primary: rgba(0, 0, 0, 0.87);
  --text-secondary: rgba(0, 0, 0, 0.6);
  --error: #d32f2f;
  --warning: #ffa000;
  --info: #0288d1;
}
```

Change these values to rebrand the whole app instantly.

Adding a New Restoration Type

1. Add an option to both <select id="project-type"> and <select id="project-type-filter">
2. Add an entry to getProjectIcon() in the script:

```js
function getProjectIcon(type) {
  const icons = {
    // existing...
    savanna: 'sun'
  };
  return icons[type] || 'leaf';
}
```

Adding a New Monitoring Type

1. Add an option to <select id="monitoring-type">
2. Add an entry to getMonitoringIcon()

Changing Default Map Centre / Zoom

In initMaps():

```js
const defaultCentre = [-25.2744, 133.7751]; // Australia
const defaultZoom = 4;
```

Replace with your region's coordinates and preferred zoom.

Increasing the Photo Size Limit

In handlePhotoUpload():

```js
if (file.size > 2 * 1024 * 1024) {  // 2 MB — change here
```

Be mindful of the localStorage quota.

---

Browser Support

Browser Minimum Version Notes
Chrome / Edge 90+ Full support
Firefox 88+ Full support
Safari 14+ Full support; check localStorage quota behaviour
Mobile Safari iOS 14+ Full support; layout responsive
Chrome Android 90+ Full support

Required APIs:

· localStorage
· FileReader
· Blob / URL.createObjectURL (for export)
· Canvas 2D (for Chart.js)
· ES6 (const/let, arrow functions, template literals, spread)

---

Troubleshooting

"My data disappeared!"

Data is scoped per origin and per browser profile. Causes:

· Opening via file:// on one machine and http://localhost on another
· Clearing browser data / using private browsing
· Different browser or user profile

Fix: Always Export Data regularly and keep JSON backups outside the browser.

"I get 'Error saving data: QuotaExceededError'"

Your localStorage quota is full (usually because of base64 photos).

Fix:

1. Export current data
2. Delete some photo-heavy records
3. Re-import the export (after pruning photos manually in a text editor if needed)

"Map tiles don't load"

The app depends on OpenStreetMap's public tile server. If blocked by a firewall, corporate proxy, or offline, maps will appear grey.

Fix: Host your own tile server (e.g. TileServer GL) and update the L.tileLayer() URLs.

"Charts are blank"

Chart.js requires the canvas to be visible when drawn. If you navigated to Reports and nothing appears:

Fix: The app auto-refreshes charts after 100 ms — try switching tabs away and back. If that fails, open DevTools and check the console for errors (often CDN loading failures).

"PDF export just downloads a tiny file"

Check that jsPDF loaded from the CDN. In DevTools console, run:

```js
window.jspdf
```

If undefined, the CDN is blocked. Download jsPDF locally and reference it with a <script src="./jspdf.umd.min.js">.

"Photos won't upload"

· Check file type (must match image/*)
· Check size (≤ 2 MB)
· Some browsers restrict FileReader on cross-origin iframes

"Import fails with 'Invalid data format'"

The JSON must contain all four top-level keys: projects, monitoringPoints, activities, teamMembers. Even empty arrays are fine, but the keys must exist.

---

Limitations & Roadmap

Current Limitations

· Single-user, single-browser — no sync, no multi-user collaboration
· No backend — data lives only in localStorage
· Milestones are read-only in the UI — can only be populated via imported JSON or by extending the code
· HTML and CSV report formats are stubs — only PDF is functional
· No offline map tiles — requires internet for map display
· No authentication or roles — all UI actions are available to anyone with the file
· Photo storage is inline base64 — inefficient for large collections
· Sample data is auto-seeded if storage is empty — no way to disable without editing code

Potential Roadmap

☐ Add/edit/delete milestones in the UI
☐ Functional HTML and CSV report generation
☐ IndexedDB backend for larger datasets and binary photo storage
☐ Optional cloud sync (e.g. via Supabase, Firebase, or a simple REST endpoint)
☐ GeoJSON import/export for GIS interoperability
☐ Offline tile caching via Service Worker
☐ Multi-language support
☐ User authentication & role-based access control
☐ Bulk CSV import for monitoring observations
☐ Photo compression before storage

---

License & Attribution

· Leaflet — BSD-2-Clause
· Chart.js — MIT
· jsPDF — MIT
· html2canvas — MIT
· Font Awesome Free — CC BY 4.0 (icons), SIL OFL 1.1 (fonts), MIT (code)
· Roboto font — Apache 2.0
· OpenStreetMap tiles — © OpenStreetMap contributors, ODbL

---

Support

For bug reports or feature requests, document the following when reporting:

1. Browser and version
2. Steps to reproduce
3. Expected vs. actual behaviour
4. Any console errors (F12 → Console tab)
5. An Export Data JSON, if the issue relates to specific records (redact sensitive info)

---

Built for the ecological restoration community — happy restoring! 🌱
