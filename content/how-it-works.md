---
title: How it works
slug: how-it-works
description: Put files in /content. Add them here. Done.
menu: Start
---

This is a single HTML file that reads markdown files with `fetch()` and renders them as static HTML. It is designed for simple personal sites on Neocities.

## The flow

1. Open `content/index.json`.
2. Add your markdown file path to the `files` array.
3. Add YAML front matter to the markdown file.
4. Refresh the page.

## Why it is simple

- There is no build step.
- There is no database.
- You control the file structure.

> If you want to keep it even simpler, remove the descriptions and let the first `#` heading be the page title.

## IndieWeb support (minimal)

Neo-CMS includes a few IndieWeb affordances without any heavy federation.

### IndieAuth identity links

Add `rel="me"` links in `index.html` so other sites can verify your identity when you sign in elsewhere. There is no login UI here.

Update `content/index.json` with your live URL so webmention targets resolve correctly:

```
"url": "https://example.com"
```

### Webmentions

Neo-CMS uses [webmention.io](https://webmention.io/) to receive inbound webmentions. Add your site on webmention.io, then paste the endpoint into `content/index.json` and `index.html`:

```
"webmentionEndpoint": "https://webmention.io/example.com/webmention"
```

```
<link rel="webmention" href="https://webmention.io/example.com/webmention" />
<link rel="pingback" href="https://webmention.io/example.com/xmlrpc" />
```

When another site links to a page, its webmention appears beneath the content as a reaction or comment.

If a page needs a custom target URL, add a `url` field to its front matter. Neo-CMS uses it when querying webmention.io.
