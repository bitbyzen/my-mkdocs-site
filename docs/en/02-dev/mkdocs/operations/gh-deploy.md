# Deploy with gh-deploy

`mkdocs gh-deploy` is a built-in MkDocs command that builds your site locally and pushes the generated files directly to the `gh-pages` branch.  
This method **does not use GitHub Actions**.

---

## Important

!!! Warning
    Do **not** use `mkdocs gh-deploy` if your repository is configured to deploy using **GitHub Actions**.  
    Choose **one deployment method** and use it consistently:

    - GitHub Actions > automated deployment on push
    - `mkdocs gh-deploy` > manual deployment from your local machine  

Mixing both methods can cause confusion.

---

## When Should You Use `gh-deploy`?

Use this method if:

* You do **not** want to use GitHub Actions
* You prefer manual deployment
* You want the simplest possible setup
* You are working on a small personal project

---

## How It Works

When you run:

```bash
mkdocs gh-deploy
```

MkDocs will:

1. Build the site locally
2. Create (or update) the `gh-pages` branch
3. Push the built site to GitHub
4. Publish it via GitHub Pages

---

## Setup Requirements

1. GitHub Pages must be configured to use:

      ```
      Deploy from a branch
      ```

2. Select:

      * Branch: `gh-pages`
      * Folder: `/ (root)`

      You can configure this in:

      ```
      Settings > Pages
      ```

---

## Deploy the Site

From the project root (where `mkdocs.yml` exists), run:

```bash
mkdocs gh-deploy
```

For a clean rebuild:

```bash
mkdocs gh-deploy --clean
```

---

## Updating the Site

After modifying your documentation:

```bash
mkdocs gh-deploy
```

That’s it.  
You do **not** need to manually:

```bash
git add .
git commit
git push
```

because `gh-deploy` handles the deployment branch automatically.

---

## How This Differs from GitHub Actions

| GitHub Actions              | `mkdocs gh-deploy`     |
| --------------------------- | ---------------------- |
| Automatic on every push     | Manual command         |
| No local build required     | Builds locally         |
| No `gh-pages` branch needed | Uses `gh-pages` branch |
| CI-based                    | Local deployment       |

---

## Deployment Timing

After running `mkdocs gh-deploy`, it may take 20–60 seconds for the site to update.  
If changes do not appear immediately, refresh the page.
