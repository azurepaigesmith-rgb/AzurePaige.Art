---
title: Neo-CMS
slug: home
description: A tiny flat-file CMS for Neocities.
menu: Start
---

Welcome to [Neo-CMS](https://github.com/CrowderSoup/Neo-CMS), a tiny flat-file CMS made for Neocities. It is a single `index.html` file that reads markdown from `/content` and renders it as static HTML in the browser.

- Drop a new `.md` file inside `/content`.
- Add it to `content/index.json`.
- Add front matter (title, slug, description, menu). Done.

---

## What you get

- Markdown rendering for headings, lists, links, and code.
- Hash-based routing so every page has a shareable URL.
- No build step, no dependencies, no database.
- Minimal IndieWeb affordances like `rel="me"` links.
- Webmentions via `webmention.io`.

> This stays intentionally small. Tweak the HTML, adjust the CSS, and make it your own.

## What this is not

- Not a full-featured CMS.
- Not WordPress, Ghost, or a site builder.
- Not meant for plugins or multi-user workflows.

## Example code

```
const words = ["plain", "fast", "cute"];
console.log(words.join(" / "));
```

## Links

Check out [Neocities](https://neocities.org) or add your own links here.
