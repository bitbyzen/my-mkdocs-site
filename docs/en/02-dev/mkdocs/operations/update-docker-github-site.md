# Update the site

## 1. Create the page file

Create the new documentation page inside the `docs/` directory:

```text
docs/outlook.md
```

MkDocs automatically detects files in `docs/`, but they will not appear in navigation unless added to `mkdocs.yml`.

---

## 2. Add the page to navigation in `mkdocs.yml`

MkDocs only shows pages in the left navigation if they are included in the `nav:` section.

Example:

```yaml
nav:
  - Home: index.md
  - Outlook: outlook.md
```

If your site is bilingual, make sure to add the page in both language configs if applicable.

---

## 3. Commit and push both the new file and the config change

Both the page file and the `mkdocs.yml` change must be committed and pushed. Otherwise, the page may exist but will not appear in navigation.

### Option 1 (most common): commit everything

```bash
git add .
git commit -m "Add Outlook page"
git push
```

---

### Option 2: stage only the files you changed

```bash
git add docs/outlook.md mkdocs.yml
git commit -m "Add Outlook page to nav"
git push
```

---

## Important

The key point is:

**`mkdocs.yml` must be committed and pushed for navigation changes to appear.**

If you updated configuration such as navigation or language links, commit and push:

```bash
git add mkdocs.yml
git commit -m "Update navigation or fix language switch links"
git push
```

---

## Result

After pushing:

* The new page will appear in the left navigation
* The site will rebuild automatically
* The updated configuration will be applied
* GitHub Pages (or your deployment target) will publish the updated site

---

## Summary

To update the site:

1. Create the page file in `docs/`
2. Add the page to `nav:` in `mkdocs.yml`
3. Commit and push both the page and the config

Configuration changes do not take effect until they are committed and pushed.
