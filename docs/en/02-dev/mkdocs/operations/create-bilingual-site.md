# Create a bilingual site

## Purpose

When creating a bilingual documentation site (English and Japanese), specific configuration steps are required to ensure proper language switching and prevent automatic configuration overrides.

This memo describes the required settings.

---

# Step 1: Turn off automatic reconfiguration

If you are using Material’s i18n switching feature, disable automatic reconfiguration to prevent the configuration from being rewritten.

Add the following to your `mkdocs.yml`:

```yaml
reconfigure_material: false
```

## Why this is required

* Prevents automatic modification of language and navigation settings
* Ensures manual bilingual configuration remains intact
* Avoids conflicts with custom language paths and alternate links

---

# Step 2: Configure alternate language links

Add the following section to define the alternate language versions of your site:

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

---

# Explanation

## generator: false

Optional but recommended.

* Removes the generator meta tag
* Produces cleaner HTML metadata

---

## alternate

Defines alternate language versions.

Each entry includes:

### name

Display label for the language:

```yaml
name: EN
name: JA
```

### link

URL path to the root of that language version.

Replace `REPO_NAME` with your repository name:

```yaml
/my-docs/
/my-docs/ja/
```

### lang

Language code:

```yaml
en
ja
```

---

# Example

```yaml
reconfigure_material: false

extra:
  generator: false
  alternate:
    - name: EN
      link: /my-docs/
      lang: en
    - name: JA
      link: /my-docs/ja/
      lang: ja
```

---

# Summary

To create a bilingual site:

1. Turn off automatic reconfiguration
2. Configure alternate language links

These settings ensure proper language switching and stable configuration behavior.
