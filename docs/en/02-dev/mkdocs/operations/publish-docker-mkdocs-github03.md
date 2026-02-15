## 7) Common updates

### A) Create the page file

```text
docs/outlook.md
```

### B) Add the page to navigation in `mkdocs.yml`

MkDocs only shows pages in the left navigation if they are included in `nav:`

### C) Commit *both* the new file and the config change

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

The key is: **`mkdocs.yml` must be committed and pushed** if you want the nav to change.

---

## 8) (If you use languages) 
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

