# neo-cms

Neo-CMS is a tiny, flat-file CMS for personal sites on Neocities. It keeps content in `content/`, renders it into static HTML in the browser, and stays intentionally small and understandable.

![Neo CMS screenshot](screenshot.png)

## Purpose

Neo-CMS exists to make a simple, learnable workflow for hobbyists and tinkerers who want to edit a few pages and publish them to Neocities without build tools, databases, or plugins.

## What it is

- A super simple CMS for Neocities
- Flat files in `content/` as the source of truth for pages
- Page metadata in YAML front matter
- Static HTML rendered from that content (no build step)
- Minimal IndieWeb support (microformats + basic semantics)
- Friendly for learners, hobbyists, and small personal sites

The menu is generated from each file's front matter, while `content/index.json` lists the files to load.

## What it is not

- Not a full-featured CMS
- Not WordPress, Ghost, or a site builder
- Not designed for multi-user workflows, plugins, or complex content models
- Not a general-purpose framework

## Supported usage paths

### A. Download a release artifact

Best if you just want to publish a site quickly and keep everything manual.

1. Download `neo-cms.zip` from the latest GitHub release.
2. Unzip it locally.
3. Edit files in `content/` and `style.css`.
4. Upload the folder to Neocities.

### B. Fork the repo

Best for people who want version control or automatic deploys. If you are a Neocities Supporter, you can use GitHub Actions to auto-deploy.

1. Fork this repo on GitHub.
2. Edit content in your fork.
3. Add a repository secret named `NEOCITIES_API_KEY` with your Neocities API key.
4. Push to `main` to deploy via GitHub Actions.

## IndieWeb support (minimal)

Neo-CMS includes basic IndieWeb affordances:

- IndieAuth `rel="me"` links
- Webmentions via `webmention.io`

Setup lives in `content/index.json` under `site.indieweb`, plus a few tags in `index.html`.

## Contributing

Keep changes small, clear, and aligned with the project goals. See `CONTRIBUTING.md` for scope and expectations.

## License

MIT. See `LICENSE`.
