# Deploying RKAS Inc. on GitHub Pages

The site is static (everything lives in `site/`) and deploys to GitHub Pages via
GitHub Actions. **Forms are in demo mode** — they validate input and run the
hCaptcha human-check, then show the confirmation, but **do not send anything
anywhere** (no backend, no email address in the code). A real handler can be
added later.

## 1. Enable GitHub Pages (one time)

GitHub → repo **Settings → Pages → Build and deployment → Source: GitHub Actions**.

That's it — the workflow in `.github/workflows/deploy.yml` publishes the `site/`
folder on every push to `main`. After the first run, your site is at:

```
https://tabalpetty.github.io/rkasinc-website/
```

(Or your custom domain — see step 3.)

## 2. Forms (demo mode)

Both the **contact form** and the **whitepaper modal** run the full front-end
flow — required-field validation, a corporate-email check (whitepaper), and an
**hCaptcha** "I'm human" widget — and then:

- **Contact form** → shows the confirmation page (`thanks.html`).
- **Whitepaper** → shows the success state and starts the PDF download.

Nothing is transmitted to any server or inbox. There is **no destination address
anywhere in the code**. When you're ready to actually capture leads, replace the
`captureLead()` no-op (whitepaper) and the contact `submit` handler in
`site/index.html` with a real backend call (e.g. a Cloudflare Pages Function that
verifies the captcha server-side).

### hCaptcha site key

1. Create a free site at **dashboard.hcaptcha.com** and copy your **site key**.
2. In `site/index.html`, find `var CAPTCHA_SITEKEY` near the top of the
   `<script>` and replace the placeholder (currently hCaptcha's public **test
   key**, which always passes) with your real site key:
   ```js
   var CAPTCHA_SITEKEY = 'your-real-hcaptcha-sitekey';
   ```
   Set it to `''` to turn the captcha off entirely.
3. Note: in demo mode the captcha is purely a front-end check — the test key
   always passes. Real (server-side) verification would need a backend.

## 3. Custom domain (optional, rkasinc.com)

1. Add a file `site/CNAME` containing one line: `rkasinc.com`
2. At your DNS provider, point the domain to GitHub Pages
   (`A` records to GitHub's IPs, or a `CNAME` to `tabalpetty.github.io`).
3. GitHub → Settings → Pages → set the custom domain and enable HTTPS.

With a root custom domain, all paths already work (the site uses relative
links throughout).

## Notes

- The PDFs are statically served and reachable by direct URL — normal for
  marketing lead-gen. GitHub Pages can't gate file access server-side.
- No build step: it's plain HTML/CSS/JS. The Action just uploads `site/`.
