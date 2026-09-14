# FAST-HEP Main Documentation Agent Instructions

This repository builds the FAST-HEP project site at
<https://fast-hep.github.io>. It is a Hugo site for project-level orientation,
not the reference documentation for every package.

## Ownership

This repository owns:

- the project-level introduction and navigation for FAST-HEP;
- the high-level package map and descriptions of package responsibilities;
- explanations of how packages interact through shared concepts and extension
  boundaries;
- shared concepts, project status, and entry points for users and contributors;
- signposting to package documentation, package repositories, and workshop
  tutorials.

This repository does not own:

- detailed package APIs or implementation documentation;
- Flow authoring, compiler, plan, runtime, or execution reference material;
- package-specific developer documentation;
- workshop tutorial content or runnable examples;
- analysis-specific documentation.

Prefer concise project-level explanations and link to the authoritative package
documentation for details. Do not duplicate package reference material here.

## Important Locations

- `config.toml` - Hugo base URL, theme selection, navbar, footer, social links,
  and project-wide navigation.
- `content/_index.md` - home page and top-level FAST-HEP overview.
- `content/getting-started.md` - project-level getting-started path that
  points users to runnable workshop material.
- `content/concepts/` - shared conceptual pages across the ecosystem.
- `content/packages/_index.md` - package map, package boundaries, and
  authoritative downstream documentation links.
- `content/tutorials/_index.md` - signpost to `fasthep-workshop` tutorials.
- `content/contributing.md` - project-site entry point for contribution
  guidance.
- `archetypes/default.md` - default Hugo front matter for new pages.
- `layouts/_markup/render-codeblock-mermaid.html` - Mermaid code-block renderer
  and shared diagram classes.
- `assets/css/custom.css` - Hugo asset-pipeline CSS customisation.
- `static/images/` - source static images copied into the generated site.
- `themes/` - vendored/submodule Hugo themes; avoid editing them for routine
  site content.
- `public/` and `resources/_gen/` - generated Hugo output and caches.

## Commands

Use Pixi from the repository root:

```bash
pixi install
pixi run serve
pixi run build
pixi run build --baseURL "https://fast-hep.github.io/"
```

`pixi run serve` runs
`hugo server --buildDrafts --noHTTPCache --poll 500ms` for local preview.
`pixi run build` runs `hugo --minify` and writes the production site to
`public/`. The Pages workflow builds with an explicit `--baseURL` supplied by
GitHub Pages; use the canonical URL above for local production-style checks.

There is currently no repository-local external link-check task. Hugo builds
validate shortcode references and internal `ref` links, but not all external
URLs. When moving or renaming content, run a production build and manually check
changed external links, or add a dedicated link-check task before documenting it
as required infrastructure.

## Front Matter and Navigation

- Use Markdown content under `content/`; edit Hugo source rather than generated
  files in `public/`.
- Prefer YAML front matter with at least `title`. Use `weight` to order pages
  inside sections such as `content/concepts/`.
- Keep top-level navigation in `config.toml` aligned with the content tree.
- Use Hugo `ref` shortcodes for internal content links when practical.
- Keep drafts clearly marked with `draft: true`; drafted pages are visible in
  local preview because the serve task builds drafts.
- Use canonical package, repository, and distribution names, for example
  `fasthep-flow`, `fasthep-carpenter`, `fasthep-curator`, `fasthep-render`,
  `fasthep-cli`, `fasthep-distributed`, `fasthep-toolbench`,
  `fasthep-workshop`, `fasthep`, and `fasthep-dev`.

## Authoritative Downstream Documentation

Link to package-owned documentation and repositories for details:

- Flow: <https://fasthep-flow.readthedocs.io/en/latest/> and
  <https://github.com/FAST-HEP/fasthep-flow>
- Carpenter: <https://fasthep-carpenter.readthedocs.io/en/latest/> and
  <https://github.com/FAST-HEP/fasthep-carpenter>
- Curator: <https://fasthep-curator.readthedocs.io/en/latest/> and
  <https://github.com/FAST-HEP/fasthep-curator>
- Render: <https://fasthep-render.readthedocs.io/en/latest/> and
  <https://github.com/FAST-HEP/fasthep-render>
- CLI: <https://fasthep-cli.readthedocs.io/en/latest/> and
  <https://github.com/FAST-HEP/fasthep-cli>
- Workshop: <https://fasthep-workshop.readthedocs.io/en/latest/> and
  <https://github.com/FAST-HEP/fasthep-workshop>
- Metapackage and contribution guide:
  <https://github.com/FAST-HEP/fasthep>
- Development workspace:
  <https://github.com/FAST-HEP/fasthep-dev>

Maintain these as signposts. If package APIs, tutorials, or developer
instructions need detailed updates, change the owning repository instead.

## Content Conventions

- Keep package descriptions and architecture diagrams consistent across the
  site.
- Distinguish released capabilities from planned or experimental work.
- Avoid presenting experimental analysis layouts as stable project contracts.
- Preserve accessible headings, descriptive link text, alt-aware images, and
  readable diagrams.
- Keep Mermaid diagrams project-level; detailed operation graphs and generated
  workflow artifacts belong in package docs or workshop tutorials.
- Verify internal and external links when moving, renaming, or deleting content.
- Keep status language current and explicit when the project is in alpha or a
  capability is still evolving.

## Deployment and Generated Output

GitHub Pages deploys the generated `public/` site from the Hugo build. Do not
edit `public/` or `resources/_gen/` by hand; change `content/`, `config.toml`,
`layouts/`, `assets/`, or `static/` instead. Static assets that are meant to be
served directly belong in `static/`; CSS processed by Hugo belongs in `assets/`.

Keep CI descriptions durable: document the local Pixi build contract and Pages
deployment boundary, not the current runner matrix or action versions.
