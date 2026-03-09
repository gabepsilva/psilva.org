# psilva.org

Static site built with [Hugo](https://gohugo.io/).

## Quick start

```bash
# Development server (with draft content)
hugo server --buildDrafts

# Development server (published content only)
hugo server

# Build site for production
hugo
```

Output is written to `public/` by default.

## Project layout

- `content/` – Markdown content (posts, pages)
- `themes/ananke/` – Ananke theme (git submodule)
- `hugo.toml` – Site config (title, baseURL, theme)
- `archetypes/` – Default front matter for new content

## New content

```bash
hugo new content posts/my-post.md
hugo new content my-page.md
```

## Theme

Using the [Ananke](https://github.com/theNewDynamic/gohugo-theme-ananke) theme. Change theme in `hugo.toml` or install another from https://themes.gohugo.io/.
