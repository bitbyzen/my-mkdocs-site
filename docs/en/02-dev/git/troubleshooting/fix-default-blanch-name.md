# Fix “Using 'master' as the name for the initial branch”

## Symptoms

Running the following command in the Ubuntu terminal inside VS Code:

```bash
git init
```

results in the following message:

```
Using 'master' as the name for the initial branch.
This default branch name is subject to change.
To configure the initial branch name to use in all of your new repositories,
which will suppress this warning, call:
git config --global init.defaultBranch <name>
Names commonly chosen instead of 'master' are 'main', 'trunk' and 'development'.
The just-created branch can be renamed via this command:
git branch -m <name>
Initialized empty Git repository in  
/home/xxx/projects/docker/python/mkdocs/.git/
```

## Cause

This message means Git has initialized the repository using the old default branch name, **`master`**.

## Solution

Rename the default branch to **`main`**, which is the current convention:

```bash
git branch -M main
```
