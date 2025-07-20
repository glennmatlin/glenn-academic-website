# Glenn's Academic Website

This is the source code for my academic and professional website, built with [Quarto](https://quarto.org/).

## Development

To work on this site locally, you'll need to have Quarto and Python installed. The Python dependencies are managed with `uv`.

1.  **Set up the environment:**

    ```bash
    uv venv
    source .venv/bin/activate
    uv pip install -r requirements.txt
    ```

2.  **Preview the site:**

    ```bash
    quarto preview
    ```

3.  **Render the site:**

    ```bash
    quarto render
    ```

## Publishing

The site is automatically published to GitHub Pages via a GitHub Actions workflow. Any push to the `main` branch will trigger a new build and deployment.

<!-- Trigger new workflow run -->