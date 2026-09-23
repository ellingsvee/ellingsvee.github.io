# Personal Website

This is the source code for my personal website and Quarto blog. The homepage showcases my projects, publications, and contact information, while the blog contains executable technical posts.

Can be viewed at: [https://ellingsvee.github.io](https://ellingsvee.github.io).

## Local development

[Install Quarto](https://quarto.org/docs/get-started/) and [uv](https://docs.astral.sh/uv/getting-started/installation/), then create the locked Python environment:

```sh
uv sync --project blog
```

Add future Python dependencies with `uv add --project blog <package>` so both `pyproject.toml` and `uv.lock` stay current.

### Regenerating the diffusion post

After editing `blog/posts/conditional_diffusion_sampling.ipynb`, regenerate the tracked Quarto source from the repository root:

```sh
quarto convert blog/posts/conditional_diffusion_sampling.ipynb \
  --output blog/posts/bayesian-sampling-conditional-diffusion/index.qmd
```

The notebook front matter contains the publication metadata and output-relative bibliography path, so no metadata edits should be needed after conversion. If the post title changes, update its link text in `index.html` as well. Then render the complete site to verify the post:

```sh
uv run --project blog --locked quarto render blog
```

From the repository root, preview the diffusion post with live reload:

```sh
uv run --project blog -- quarto preview blog/posts/bayesian-sampling-conditional-diffusion/index.qmd
```

Render the production blog to `blog/_site`:

```sh
uv run --project blog -- quarto render blog
```

Posts live at `blog/posts/<slug>/index.qmd`. Add each post link to the Blog section under Other in `index.html` so it appears directly on the homepage.

## Publishing

The GitHub Actions workflow renders the executable blog, combines it with the static homepage and files, and deploys the result to GitHub Pages on pushes to `main` and manual runs. Pull requests run the same render and assembly checks without deploying. In the repository's Pages settings, the publishing source must be set to **GitHub Actions**.
