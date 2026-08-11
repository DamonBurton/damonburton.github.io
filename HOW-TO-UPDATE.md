# How to Update Your Portfolio

Everything you'll routinely change lives in **`data/projects.json`** and the **`images/`** folder.
You should almost never need to open `index.html` or `project.html` themselves.

---

## 1. Adding a new project

Open `data/projects.json`. It's a list of project "blocks" between `{ }`. Copy an existing
block, paste it as a new entry (don't forget a comma between blocks), and edit the fields:

```json
{
  "slug": "url-safe-id-no-spaces",
  "title": "Short name shown in the top bar",
  "status": "Complete",
  "statusType": "complete",
  "name": "Bigger display name for the project",
  "description": "1-2 sentence summary of what it does.",
  "highlightLabel": "Design note —",
  "highlight": "The one interesting problem/debugging story worth telling.",
  "image": "images/your-photo.jpg",
  "specs": [
    { "key": "MCU", "value": "Arduino Nano" },
    { "key": "Sensor", "value": "DS18B20" }
  ],
  "writeup": [
    "First paragraph of the detailed writeup.",
    "Second paragraph. Add as many as you want — each string in this list becomes its own paragraph."
  ],
  "gallery": [
    "images/project-photo-2.jpg",
    "images/project-photo-3.jpg"
  ],
  "links": [
    { "label": "GitHub Repo", "url": "https://github.com/..." }
  ]
}
```

**Field notes:**
- `slug` — a short, unique, URL-safe identifier (lowercase, hyphens, no spaces). This is what
  the clickable card links to — e.g. `"slug": "pill-dispenser"` makes the detail page live at
  `project.html?id=pill-dispenser`. Every project needs a unique one.
- `status` / `statusType` — use `"statusType": "progress"` (shows amber) if a project is still in
  progress, or `"complete"` (shows cyan) if it's done. `status` is the actual text shown — it can say
  anything you want (e.g. `"In Progress"`, `"UBC Physics Olympics 2026"`, `"v2 — rebuilding"`).
- `specs` — add or remove as many `{ "key": ..., "value": ... }` rows as you want. These render as
  the datasheet-style list on both the card and the detail page sidebar.
- `writeup` — the longer explanation shown on the project's detail page. Each string in the list
  is its own paragraph — write as many or as few as you want. Leave as an empty list `[]` if you
  don't have a writeup yet; the section just won't render.
- `gallery` — extra photos (beyond the one main `image`) shown in a grid on the detail page.
  Leave as `[]` if you don't have extras yet.
- `links` — optional external links shown in the detail page sidebar (GitHub repo, a datasheet,
  a demo video, etc.). Leave as `[]` if not needed.
- `image` — see section 3 below.

Save the file. That's it — no HTML editing needed. The card on the homepage and its detail page
both pull from this same entry automatically.

---

## 2. How the detail pages work

Clicking any project card takes you to `project.html?id=<slug>` — this is a single template file
that reads the `id` from the URL, looks up the matching project in `projects.json`, and builds the
page from that data. This means you never create a new HTML file per project — adding a project to
the JSON automatically gives it a working detail page, and deleting one from the JSON removes its
detail page too (there's no separate file to clean up).

---

## 3. Adding your own photos (replacing the placeholder graphics)

1. Drop your photo file into the `images/` folder (jpg or png both work).
   Keep filenames simple, no spaces — e.g. `pill-dispenser-final.jpg`.
2. In `data/projects.json`, find the project's `"image"` field and point it at your new file
   (this is the main photo shown on the card and at the top of the detail page):
   ```json
   "image": "images/pill-dispenser-final.jpg"
   ```
3. To add extra photos that only show up on the detail page (build progress shots, close-ups,
   etc.), add file paths to that project's `"gallery"` list:
   ```json
   "gallery": ["images/pill-dispenser-build.jpg", "images/pill-dispenser-closeup.jpg"]
   ```
4. Landscape photos work best — the image areas are wide banners. A slightly cropped photo still
   looks fine; the site crops to fill automatically.

If you leave a project's `image` field pointing at the placeholder `.svg` files, that's fine too —
they're just simple line-art stand-ins so the site doesn't look broken before you add real photos.

---

## 4. Removing a project

Delete its whole `{ ... }` block from `data/projects.json`. Make sure you also delete the comma
that separated it from the next block (JSON is picky about trailing/missing commas).

---

## 5. Editing Skills, Education, or Awards

These sections are currently written directly into `index.html` (they change less often than
projects, so they weren't worth making a separate data file for). To edit them:

- **Skills** — search `index.html` for `id="skills"`, then find the `<ul>...</ul>` list under the
  category you want to change and edit the `<li>` lines directly.
- **Background/Education/Awards** — search for `id="background"`, then edit the text inside the
  `<h4>`, `.meta`, and `<p>` tags under each entry.

If you'd like these made data-driven too (same JSON approach as projects), just ask — it's the
same pattern, just more files to keep track of.

---

## 6. Changing colors

Styles now live in **`css/styles.css`** (not inside `index.html` anymore — it's shared between
the homepage and the project detail pages). Near the top of that file there's a `:root { ... }`
section with named color variables (`--bg`, `--green` [now cyan], `--ink`, etc.). Changing a hex
value there updates it everywhere on the site automatically — homepage and detail pages both.

---

## 7. Testing changes before pushing to GitHub

Because the site loads `projects.json` dynamically, double-clicking `index.html` (or `project.html`)
to preview it locally **will not work** — browsers block that for security reasons, and you'll see
a "couldn't load projects" message instead of your cards.

To preview locally: open a terminal in the `portfolio` folder and run:
```
python3 -m http.server
```
then open `http://localhost:8000` in your browser. Once you push to GitHub Pages, this issue goes
away entirely — GitHub Pages serves it as a real website, so the fetch works normally.

---

## 8. Publishing / updating on GitHub Pages

Every time you make changes:
1. Commit and push the changed files to your GitHub repo (the whole `portfolio` folder —
   `index.html`, `project.html`, `css/`, `data/`, and `images/`).
2. GitHub Pages rebuilds automatically within a minute or two — no separate deploy step.

```
git add .
git commit -m "describe what you changed"
git push
```
