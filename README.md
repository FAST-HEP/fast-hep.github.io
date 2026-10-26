# FAST-HEP project site

This repository builds the FAST-HEP project-level documentation at
<https://fast-hep.github.io/>. Package reference documentation and runnable
tutorials remain in their owning repositories.

## Building the site

Use the repository's Pixi environment:

```bash
pixi install
pixi run serve
```

The local preview is available at <http://localhost:1313/>. Build the
production site with:

```bash
pixi run build --baseURL "https://fast-hep.github.io/"
```

The generated site is written to `public/`. Edit the sources under `content/`,
`layouts/`, `assets/`, and `static/`; do not edit `public/` or
`resources/_gen/` by hand.

## Theme

The site uses the
[`scientific-python-hugo-theme`](https://github.com/scientific-python/scientific-python-hugo-theme),
configured in `config.toml`.
