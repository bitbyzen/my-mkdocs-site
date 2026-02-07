# Fix “Author identity unknown”

## Symptoms

Running the following command in the Ubuntu terminal inside VS Code:

```bash
git commit -m "Initial MkDocs site"
```

results in the following message:

```
Author identity unknown

*** Please tell me who you are.

Run

  git config --global user.email "you@example.com"
  git config --global user.name "Your Name"

to set your account's default identity.
Omit --global to set the identity only in this repository.

fatal: empty ident name (for <kuroraiun@Daiv-001.localdomain>) not allowed
```

## Solution

Set your Git identity.

* `you@example.com`: The same email address you use on GitHub
* `Your Name`: Any name (usually your real name)

Example:

```bash
git config --global user.name "Alex Tanaka"
git config --global user.email "alex@email.com"
```
