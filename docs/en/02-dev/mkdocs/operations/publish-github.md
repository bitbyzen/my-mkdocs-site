# Publish a MkDocs website using GitHub Pages

1. Connect your project to GitHub.

    ```bash title="terminal"
    git init
    git add .
    git commit -m "Initial MkDocs site"
    git branch -M main
    git remote add origin https://github.com/yourusername/your-repo-name.git
    git push -u origin main
    ```

2. Deploy to GitHub Pages.

    ```bash title="terminal"
    mkdocs gh-deploy
    ```

    !!! Note
        This command:

        - Builds your site
        - Creates a `gh-pages` branch
        - Pushes the static HTML to that branch
        - GitHub Pages serves it automatically

3. Enable GitHub Pages in your GitHub repository:  
    Go to **Settings** > **Pages**, then set:

    * **Source:** Deploy from a branch
    * **Branch:** `gh-pages` / `root`

4. Your site will be available at:

    ```
    https://yourusername.github.io/your-repo-name/
    ```
