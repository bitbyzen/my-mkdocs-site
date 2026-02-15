# Publish a Docker-based MkDocs site to GitHub Pages (step-by-step)

This guide assumes you build and preview MkDocs **inside Docker**, but publish to **GitHub Pages** using **GitHub Actions**.

---

## Prerequisites

- A GitHub account
- A MkDocs project folder that contains:
  - `mkdocs.yml`
  - `docs/` (your Markdown pages)
- Docker installed locally (for preview/build)
- Git installed locally (for pushing to GitHub)

---

## 1) Create your GitHub repository

1. Log in to GitHub.
2. Create a new repository (**New repository**), then set:
   - **Repository name**: anything (example: `kb`)
   - **Public/Private**: **Public** (required for free GitHub Pages on personal accounts)
   - **Initialize with README**: unchecked (optional)
   - **.gitignore / License**: optional

> Your Pages URL will be:
>
> - **Project pages (most common):** `https://YOUR_USERNAME.github.io/REPO_NAME/`
> - **User/Org pages:** `https://YOUR_USERNAME.github.io/` (requires a repo named `YOUR_USERNAME.github.io`)

This guide uses **project pages**: `https://YOUR_USERNAME.github.io/REPO_NAME/`.

---

## 2) (Recommended) Add a Dockerfile for MkDocs

In your project root (same folder as `mkdocs.yml`), create `Dockerfile`:

```dockerfile
FROM python:3.12-slim

WORKDIR /mkdocs

# Install MkDocs + your theme/plugins
RUN pip install --no-cache-dir mkdocs mkdocs-material

# Default command (can be overridden)
CMD ["mkdocs", "--version"]
```

Build the image:

```bash
docker build -t my-mkdocs .
```

> If you already have an image, keep using it (just replace `my-mkdocs` with your image name in the commands below).

---

## 3) Preview the site locally (Docker)

From the project root:

```bash
docker run --rm -it -p 8000:8000 -v "$(pwd)":/mkdocs my-mkdocs   mkdocs serve -a 0.0.0.0:8000
```

Open: `http://localhost:8000`

---

## 4) Initialize git and push your source to GitHub

From your project root:

```bash
git init
git add .
git commit -m "Initial MkDocs site"
git branch -M main
git remote add origin https://github.com/YOUR_USERNAME/REPO_NAME.git
git push -u origin main
```

If you already ran `git init`, skip it.

If `origin` already exists, update it:

```bash
git remote set-url origin https://github.com/YOUR_USERNAME/REPO_NAME.git
```

### Authentication note (if Git asks for a password)
GitHub no longer accepts account passwords for HTTPS git pushes. Use a **Personal Access Token (PAT)** as the password.

---

## 5) Add a GitHub Actions workflow to build + deploy

Create the workflow folder:

```bash
mkdir -p .github/workflows
```

Create `.github/workflows/deploy.yml`:

```bash
nano .github/workflows/deploy.yml
```

Paste this workflow:

```yaml
name: Deploy MkDocs (GitHub Pages)

on:
  push:
    branches: ["main"]
  workflow_dispatch:

permissions:
  contents: read
  pages: write
  id-token: write

concurrency:
  group: "pages"
  cancel-in-progress: true

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4

      # Option A (simple): install python + mkdocs in the runner
      - name: Set up Python
        uses: actions/setup-python@v5
        with:
          python-version: "3.x"

      - name: Install MkDocs
        run: |
          pip install mkdocs mkdocs-material

      - name: Build
        run: mkdocs build --clean

      - name: Upload Pages artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: site

  deploy:
    needs: build
    runs-on: ubuntu-latest
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
```

> If you **don’t** use `mkdocs-material`, change the install line to `pip install mkdocs`.
>
> If you use additional plugins, add them to the install step, e.g.:
> `pip install mkdocs-material mkdocs-static-i18n`

Commit and push:

```bash
git add .github/workflows/deploy.yml
git commit -m "Add GitHub Pages deployment workflow"
git push
```

---

## 6) Enable GitHub Pages (use GitHub Actions as the source)

In your GitHub repo:

1. **Settings → Pages**
2. Under **Build and deployment → Source**
3. Select **GitHub Actions**

Now go to the repo’s **Actions** tab and confirm the workflow run completes (green ✅).

Your site should be live at:

```text
https://YOUR_USERNAME.github.io/REPO_NAME/
```

---

## 7) Common updates: add a new page and make it show up in the left nav

### A) Create the page file

Example:

```text
docs/outlook.md
```

### B) Add the page to navigation in `mkdocs.yml`

MkDocs only shows pages in the left navigation if they are included in `nav:` (or if you’re using a plugin/config that auto-generates nav).

Example (single-language):

```yaml
nav:
  - Home: index.md
  - Outlook: outlook.md
```

Example (if you maintain separate nav blocks, e.g. `nav_en` / `nav_ja` via a plugin):

```yaml
nav_en:
  - Home: index.md
  - Outlook: outlook.md

nav_ja:
  - ホーム: index.md
  - Outlook: outlook.md
```

### C) Commit *both* the new file and the config change

If you created `docs/outlook.md` **and** edited `mkdocs.yml`, you can do either of these:

**Option 1 (most common): commit everything**
```bash
git add .
git commit -m "Add Outlook page"
git push
```

**Option 2: stage only the files you changed**
```bash
git add docs/outlook.md mkdocs.yml
git commit -m "Add Outlook page to nav"
git push
```

Either works. The key is: **`mkdocs.yml` must be committed and pushed** if you want the nav to change.

---

## 8) (If you use languages) Fix “Page Not Found” when switching languages on GitHub Pages

If your site is hosted at a **subpath** like `/REPO_NAME/`, language switchers sometimes generate links like `/ja/` (missing the repo prefix), causing 404.

### Reliable fix: define correct language links explicitly

1) Turn off auto reconfigure (if you’re using Material’s i18n switching feature that rewrites config):

```yaml
reconfigure_material: false
```

2) Add `extra.alternate` with the correct repo path:

```yaml
extra:
  generator: false
  alternate:
    - name: EN
      link: /REPO_NAME/
      lang: en
    - name: JA
      link: /REPO_NAME/ja/
      lang: ja
```

Commit and push:

```bash
git add mkdocs.yml
git commit -m "Fix language switch links for GitHub Pages subpath"
git push
```

---

## 9) Troubleshooting checklist

- **Nav item missing:** you probably didn’t update/commit `mkdocs.yml` (or the correct nav block).
- **Site still shows old content:** hard refresh your browser, or open an incognito window.
- **Workflow failed:** GitHub repo → **Actions** → open the failed run → check the error log.
- **404 right after rename:** GitHub Pages can take a minute to republish after a repo rename.

---

## Quick reference commands

Build:
```bash
docker run --rm -it -v "$(pwd)":/mkdocs my-mkdocs mkdocs build
```

Serve:
```bash
docker run --rm -it -p 8000:8000 -v "$(pwd)":/mkdocs my-mkdocs mkdocs serve -a 0.0.0.0:8000
```

Publish changes:
```bash
git add .
git commit -m "Update docs"
git push
```
