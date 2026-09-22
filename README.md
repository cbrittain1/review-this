# review-this

Standalone, self-contained HTML pages ("applets") used for interactive review
of in-progress materials — e.g. survey drafts, planning tools, and other
one-off interactive documents. This repository is intentionally simple:
plain HTML/CSS/JavaScript, no build step, no framework, no dependencies.

This repo is deliberately kept separate from any source content repository
so that only these standalone review pages are ever published or hosted — no
internal team documents, source materials, or private inputs live here.

## Folder structure

```
/
├── index.html                  ← landing page, links to every applet
├── applets/
│   └── <applet-name>/
│       └── index.html          ← the applet itself (self-contained)
└── README.md
```

Each applet is a single folder under `applets/` containing one `index.html`
file with all of its CSS and JavaScript inline (no external files, no
external requests). This keeps every applet easy to open, edit, and reason
about on its own.

## Adding a new applet

1. Create a new folder under `applets/`, named for the applet
   (lowercase, hyphen-separated, e.g. `applets/session-3-debrief/`).
2. Place a single `index.html` file in that folder with all styles and
   scripts inline.
3. Add a link to it from the root `index.html` landing page, in the
   `<ul class="applet-list">` list.
4. Test locally (see below) before committing.

## Updating an existing applet

Just edit the `index.html` file directly — it's plain HTML/CSS/JS, no build
step required. Re-test locally, then commit.

## Testing locally

Because these are plain static files, you can open them directly in a
browser by double-clicking `index.html`, or serve them locally to more
closely match how GitHub Pages will serve them:

```powershell
# from the repo root
python -m http.server 8080
# then open http://localhost:8080 in a browser
```

For each applet, manually verify in the browser:
- All links on the landing page navigate to the correct applet.
- Every checkbox toggles correctly and reflects the intended selection rules
  (e.g. mutually-exclusive "none of these" options, "select up to N" limits).
- Any text fields (open-response boxes, "other" fields) accept input and are
  enabled/disabled at the right times.
- Any live status text (e.g. "X of 2 selected") updates as selections change.
- The page renders correctly at both desktop and mobile widths.

## Publishing

This repo has no build step — whatever is committed to the default branch is
what gets published as-is.

**Public hosting (GitHub Pages):**
1. Push this repository to GitHub.
2. In the repo settings, under **Pages**, set the source to the default
   branch (root folder).
3. GitHub will publish the site at
   `https://<org-or-user>.github.io/expand-ai-html-applets/`.
4. Requires: a GitHub account/organization with permission to create the
   repository and change its Pages settings (repo admin access).

**Restricted hosting (Azure Static Web Apps + Microsoft Entra ID):**
If pages must be limited to organizational sign-in instead of being public,
do not use GitHub Pages. Instead:
1. Create an Azure Static Web App resource (Free tier is sufficient for
   static HTML with no API) linked to this repository.
2. Configure `staticwebapp.config.json` to require authentication
   (`"allowedRoles": ["authenticated"]` on routes) and set the Entra ID
   (Azure AD) identity provider as the default login provider.
3. Requires: an Azure subscription with permission to create resources, and
   an Entra ID tenant admin (or delegated permission) to register/consent
   the app for sign-in.

Do not enable either hosting path until you've confirmed which access model
(public vs. sign-in-required) is approved — see `decisions.md`/project
instructions in the content hub for the current decision.

## What does NOT belong in this repo

- No team documents, source materials, private inputs, or drafts from the
  content hub.
- No analytics, tracking scripts, telemetry, or third-party embeds.
- No response collection/back-end — these are static review pages only,
  not live data-collecting surveys.
- No build tooling or frameworks unless a specific applet genuinely
  requires one (none currently do).
