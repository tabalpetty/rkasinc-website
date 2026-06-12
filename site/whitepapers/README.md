# Whitepapers

Gated PDF whitepapers presented on the site (the `#whitepapers` section in
`../index.html`). Each is shown as **Title → The Challenge (hook) → Executive
Summary**, with a **Download PDF** button. Downloading requires a **corporate
email**: the visitor enters their work email, the lead is submitted via
**FormSubmit** (AJAX) to your configured address, and the PDF download starts
automatically. Free email providers (Gmail, Outlook, Yahoo, etc.) are rejected
client-side.

The PDF files live in **`../assets/`** (e.g. `assets/RKAS_Inc_WP01_RAG_Faithfulness.pdf`).

## Add a whitepaper

1. **Drop the PDF in `site/assets/`** with a clean, hyphenated/underscored name.

2. **Edit the Whitepapers section** in `../index.html` (search for
   `<!-- Whitepapers -->`). Duplicate one `.wp-card` block and update:
   - `.wp-cat` — number + category, e.g. `Whitepaper 11 · AI Governance`
   - `<h3>` — the whitepaper title
   - `.wp-challenge` — the one- or two-sentence **hook** (keep the
     `<span class="wp-label">The Challenge</span>`)
   - `.wp-summary` — the **executive summary** (keep the
     `<span class="wp-label">Executive Summary</span>`)
   - `.wp-specs` — page count, e.g. `PDF · 3 pages`
   - the `.wp-download` button's data attributes:
     - `data-pdf="assets/RKAS_Inc_WP11_My_Topic.pdf"`
     - `data-title="Short Title For The Lead Record"`

The email gate, validation, hCaptcha check, and auto-download are wired
automatically to any button with class `wp-download`.

## Where the leads go (demo mode)

**Nowhere — by design, for now.** The modal validates the corporate email and
runs the hCaptcha check, then starts the download, but it does **not** send the
lead anywhere. There is no backend and no email address in the code. To start
capturing leads later, replace the `captureLead()` no-op in `../index.html` with
a real backend call (e.g. a Cloudflare Pages Function).

## Notes / limitations

- **Client-side soft gate.** The PDFs in `site/assets/` are served statically
  and are reachable by direct URL — standard for marketing lead-gen. (True
  enforcement would need a server/serverless function issuing signed URLs, which
  GitHub Pages can't do on its own.)
- The hCaptcha widget is a front-end check (the test key always passes); real
  server-side verification would need a backend.
- To tune which domains count as "free / personal", edit the `FREE_DOMAINS`
  list in the `<script>` at the bottom of `../index.html`.
