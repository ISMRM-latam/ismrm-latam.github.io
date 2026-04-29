# ISMRM-LATAM — Website Repository

Source code for the official website of the **ISMRM Latin American Chapter**,  
hosted at [ismrm-latam.org](https://ismrm-latam.org).

> **This README is for site administrators and contributors.**  
> For information about the chapter, visit the website directly.

---

## Tech Stack

| Layer | Technology |
|---|---|
| Generator | [Jekyll](https://jekyllrb.com/) via GitHub Pages |
| Theme | [`pages-themes/minimal@v0.2.0`](https://github.com/pages-themes/minimal) (remote) |
| Hosting | GitHub Pages (auto-deploys from `main`) |
| Custom domain | `ismrm-latam.org` (configured via `CNAME`) |
| Automation | GitHub Actions (5 workflows) |

---

## Repository Structure

```text
├── _config.yml              # Jekyll configuration & site identity
├── _data/                   # All site content as JSON (single source of truth)
│   ├── content.json         # Navigation, welcome message, committee, history
│   ├── events.json          # Event listing for the Events page & homepage widget
│   ├── jobs.json            # Job/opportunity listings
│   ├── labs.json            # Research lab listings
│   ├── members.json         # Member map data (name, institution, lat/lng, etc.)
│   └── news.json            # News items for the News page & homepage widget
├── _events/                 # Jekyll collection: one .md per event (full detail pages)
├── _layouts/
│   ├── default.html         # Base layout (header, nav, footer)
│   ├── event.html           # Layout for individual event detail pages
│   └── post.html            # Layout for news/post detail pages
├── _posts/                  # Jekyll posts (news articles with full content)
├── assets/
│   ├── css/style.css        # All custom CSS
│   └── images/              # Logo and other static images
├── .github/
│   ├── ISSUE_TEMPLATE/      # Forms used by the public to submit content
│   └── workflows/           # GitHub Actions that process form submissions
├── index.html               # Homepage
├── events.html              # Events listing page
├── jobs.html                # Opportunities listing page
├── labs.html                # Research hubs listing page
├── members.html             # Interactive member map (Leaflet.js)
├── news.html                # News listing page
├── about.md                 # About page (mission, history, committee, bylaws)
└── CNAME                    # Custom domain configuration
```

---

## How Content Is Managed

Content is driven entirely by the JSON files in `_data/`. There is no CMS —  
updates are made by editing these files directly or via the automated  
GitHub Actions workflows described below.

### Members — `_data/members.json`

Each member is an object with the following fields:

```json
{
  "name": "Full Name",
  "institution": "University or Organization",
  "residence": "Country of residence (in English)",
  "country": "Country of origin",
  "lat": 0.0,
  "lng": 0.0,
  "interests": "Area 1, Area 2, Area 3",
  "bio": "Short biography."
}
```

> **Map note:** `lat`/`lng` are decimal coordinates for the member's  
> current residence. The workflow attempts to look for the correct `lat`/`lng`
> before committing. 

New members submit via the **Member Registration** issue form. The  
`member-update.yml` workflow appends the entry to `members.json`  
and commits it automatically.

---

### Events — `_data/events.json` + `_events/*.md`

Events have **two parts**:

1. **`_data/events.json`** — short metadata entry used by the Events page  
   listing and the homepage widget:

```json
{
  "title": "Event Title",
  "category": "Business Meeting",
  "event_date": "YYYY-MM-DD",
  "modality": "Virtual | In-Person | Hybrid",
  "location": "City or 'Virtual'",
  "link": "https://... or #",
  "url": "events/YYYY-MM-DD-slug/"
}
```

2. **`_events/YYYY-MM-DD-slug.md`** — full detail page with layout `event`  
   and matching front matter (`title`, `event_date`, `location`, `link`).  
   The body is free-form Markdown.

> The `url` field in `events.json` must match the filename in `_events/`  
> for the "Details" button to work correctly.

New events are submitted via the **Event Registration** issue form.  
The `event_update.yml` workflow creates both the JSON entry and the  
`_events/` file in a single commit.

---

### News — `_data/news.json` + `_posts/*.md`

Similar two-part structure:

1. **`_data/news.json`** — short metadata (title, date, summary, url)  
   for the News page listing and homepage widget.
2. **`_posts/YYYY-MM-DD-slug.md`** — full article with layout `post`.

New news items are submitted via the **News Registration** issue form,  
processed by `news-update.yml`.

---

### Labs — `_data/labs.json`

Edited directly or via the **Lab Registration** form (`lab-update.yml`).

---

### Jobs & Opportunities — `_data/jobs.json`

Edited directly or via the **Job Registration** form (`job-update.yml`).

---

### Site Text & Navigation — `_data/content.json`

Controls the navigation bar, welcome message, committee table, and  
history paragraph shown on `about.md`. Edit directly for any textual  
or structural change.

**Active fields:**

| Key | Used by |
|---|---|
| `navigation` | `_layouts/default.html` — renders the nav bar |
| `home.mission_statement` | `about.md` |
| `committee` | `about.md` |
| `history.content` | `about.md` |
| `welcome_message.*` | `index.html` |

---

## Automated Workflows (GitHub Actions)

All workflows are triggered when a **labeled issue as** `appproved` by an  
admin. The label signals the workflow to read the issue form  
data and commits the update to `main`.

| Workflow | Trigger label | Updates |
|---|---|---|
| `member-update.yml` | `approved` | `_data/members.json` |
| `event_update.yml` | `approved` | `_data/events.json` + `_events/*.md` |
| `news-update.yml` | `approved` | `_data/news.json` + `_posts/*.md` |
| `lab-update.yml` | `approved` | `_data/labs.json` |
| `job-update.yml` | `approved` | `_data/jobs.json` |

**Admin approval flow:**

1. A user submits content via an issue form on GitHub.  
2. An admin reviews the issue and **approves it** by labeling the issue with `approved`.  
3. The corresponding workflow fires, parses the issue body, closes the issue, and commits  
   the new entry to `main`.  
4. GitHub Pages automatically rebuilds and deploys the site (~1–2 min).

---

## Making Direct Edits

For quick fixes (typos, coordinate corrections, committee updates),  
edit the relevant `_data/*.json` file directly on GitHub or via a  
local clone and pull request.

**Required fields to keep valid JSON:**

- All string values must be quoted.  
- Arrays must have a trailing entry without a trailing comma.  
- Validate with [jsonlint.com](https://jsonlint.com/) before pushing.

---


## Contact

For site-related issues, open a GitHub Issue or contact the  
Chapter at  
[contacto@ismrm-latam.org](mailto:contacto@ismrm-latam.org).
