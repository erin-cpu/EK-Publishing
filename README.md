# E&K Publishing — website

The one-page site for E&K Publishing. Plain HTML and CSS with one small script. No build step,
no package manager, nothing to install.

## Files

| File | What it is |
| --- | --- |
| `index.html` | The whole site — markup, styles, and the script behind the "Copy address" button. |
| `logo.webp` | The E&K mark. Used in the hero and the footer. |
| `CNAME` | The custom domain, `publishwithek.com`. GitHub Pages reads this. |

The logo is black line art on a white background. It is not transparent. The page handles that with
CSS blend modes (`multiply` on the paper sections, `screen` with an inversion on the navy bands), so
the mark reads correctly on both without needing a second file.

## Working on it locally

Open `index.html` in a browser. That is the entire workflow.

If you want a local server instead — useful because fonts and relative paths behave exactly as they
will in production:

```bash
python -m http.server 8000
```

Then visit <http://localhost:8000>.

## Publishing with GitHub Pages

1. Push this repository to GitHub.
2. In the repository, go to **Settings → Pages**.
3. Under **Source**, choose **Deploy from a branch**, then branch `main` and folder `/ (root)`.
4. Save. The site is live at `https://<your-username>.github.io/<repo-name>/` in a minute or two.

### The custom domain

The site is configured for **publishwithek.com**. The `CNAME` file at the root of this repository is
what tells GitHub Pages to serve it there — leave it in place. GitHub also writes that same value
into **Settings → Pages → Custom domain**, so the two stay in sync.

At the registrar, point the apex domain at GitHub with four `A` records, all on host `@`:

```
185.199.108.153
185.199.109.153
185.199.110.153
185.199.111.153
```

Confirm those addresses against GitHub's own Pages documentation before you enter them — they are
stable but they are GitHub's to change.

Add one more record so the `www` version works too: a `CNAME` on host `www` pointing at
`<your-username>.github.io`.

DNS takes anywhere from a few minutes to a few hours. Once it resolves, tick **Enforce HTTPS** on the
Pages settings screen. The certificate is issued automatically and is free; the checkbox stays greyed
out until DNS has propagated, which is normal and not a sign anything is broken.

## Check these before it goes public

- **The link preview image is the logo.** `og:image` points at `logo.webp`, which is the mark on a
  white field. It works, but a photograph or a designed card reads better in iMessage, LinkedIn and
  Slack. Swap the file and keep the tag pointing at an absolute `https://publishwithek.com/` URL.
- **"Est. 2018" in the footer** was derived from "eight years in business." Confirm the year.
- **The contact address** is `Erin@estudio.events`. It appears twice in the contact section — once as
  the visible text and once in the `mailto:` link and the button's `data-email` attribute. Change all
  three together if it ever moves to a dedicated address.
- **The premise section** makes a pointed argument about traditional publishing. Intentional, but
  worth a read before strangers see it.

## Brand

| | |
| --- | --- |
| Deep Ink Navy | `#182B3A` |
| Antique Gold | `#B69A62` |

Display face is **Bodoni Moda**, body face is **Spectral**, both from Google Fonts and both with real
serif fallbacks so the page survives a blocked font request.

## Editing the page

Every color is a CSS custom property declared in the `:root` block at the top of the `<style>`.
Change a value there and it changes everywhere.

Dark mode is handled at the token level in three places, and all three need to stay in sync:

1. `:root` — the light values.
2. `@media (prefers-color-scheme: dark)` — for visitors whose system is set to dark.
3. `:root[data-theme="dark"]` — for an explicit toggle.

Add a new color by declaring it in `:root` first. A token that only exists inside one of the dark
blocks will be undefined for most visitors.

---

Site content © E&K Publishing. Erin Neuhardt & Kevin LeBlanc.
