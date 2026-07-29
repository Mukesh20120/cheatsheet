# Cheatsheets

Single-page recall sheets. Plain static HTML — no build step, no dependencies, no framework.

```
.
├── index.html          # the shelf — lists every sheet
├── sheets/
│   └── github-actions.html
└── README.md
```

Each sheet is fully self-contained: its own CSS and JS live inside the file. You can open one
straight off disk, email it to someone, or print it to PDF, and it still works.

---

## Adding a cheatsheet

**1.** Drop the `.html` file into `sheets/`. Lowercase, hyphenated:

```
sheets/docker.html
```

**2.** Open `index.html`, find the `TEMPLATE` comment inside `<div class="shelf">`, copy that
block above the comment, and fill in four things:

```html
<a class="sheet" href="sheets/docker.html">
  <span class="idx">02</span>
  <span class="meat">
    <h2>Docker</h2>
    <p>Images, layers, volumes, networks, compose, multi-stage builds.</p>
    <span class="tags"><span>Containers</span><span>DevOps</span></span>
  </span>
  <span class="side">~45 cards<span class="arrow">&rarr;</span></span>
</a>
```

The `<p>` text is what the filter box searches, so put the keywords you'd actually type.

**3.** Commit and push.

### Placeholder for a sheet you haven't written yet

```html
<span class="sheet soon">
  <span class="idx">03</span>
  <span class="meat"><h2>Kubernetes</h2><p>Coming.</p></span>
  <span class="side">Planned</span>
</span>
```

Note it's a `<span>`, not an `<a>` — nothing to link to yet.

---

## Deploying to GitHub Pages

**Settings → Pages → Build and deployment**

- Source: **Deploy from a branch**
- Branch: `main`, folder: `/ (root)`

Save. Live at `https://<username>.github.io/<repo>/` in a minute or two, and it redeploys on
every push to `main`.

### Optional: deploy via Actions instead

Only worth it if you later add a build step or want to run checks before publishing. There's a
starter workflow in `.github/workflows/deploy.yml` — switch **Source** to *GitHub Actions* to use it.

---

## Sheet house style

If you want new sheets to match, reuse these tokens:

| Token | Value | Use |
|---|---|---|
| `--paper` | `#E7E9E1` | page background |
| `--card` | `#FBFBF7` | card background |
| `--ink` | `#171B17` | body text |
| `--ink-2` | `#5C6359` | secondary text |
| `--rule` | `#CBCFC1` | borders |
| `--volt` | `#4B31C8` | accent |
| `--code-bg` | `#141914` | code blocks |

Type: **Archivo** for headings, **JetBrains Mono** for code and labels.

Conventions worth keeping: a filter box wired to `/`, `Esc` to clear, a print stylesheet, and a
`← All cheatsheets` link back to `index.html`.
