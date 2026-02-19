# Navigate to the repository root

Use a Git command to move directly to the top-level directory of the current repository, regardless of your current depth.

## Steps

1. Open a terminal inside a Git repository.
2. Run:

    ```
    cd "$(git rev-parse --show-toplevel)"
    ```

3. Verify the location:

    ```
    pwd
    ```

You are now in the root directory of the repository.

## How it works

* `git rev-parse --show-toplevel` prints the absolute path of the repository root.
* `$(...)` performs command substitution and inserts the output into the `cd` command.
* Quotes ensure paths containing spaces are handled correctly.

## Optional: create a shortcut alias

Add the following to `~/.bashrc` or `~/.zshrc`:

```
alias gr='cd "$(git rev-parse --show-toplevel)"'
```

Reload your shell:

```
source ~/.bashrc
```

Use:

```
gr
```

## Troubleshooting

If you see:

```
fatal: not a git repository
```

You are not inside a Git working tree. Navigate into a repository directory and run the command again.
