---
layout: post
title: "Project Web Viewer — a local file browser for development containers"
date: 2026-05-16
---

A self-contained Flask application that turns any project directory into a
browsable web interface with syntax-highlighted source code, rendered markdown
documents, and a plot gallery. Designed to run inside a development container
and be accessed from a Mac browser via the container IP.

## Directory structure

```
web/
├── app.py              # Flask application — routes, file serving, tree API
├── templates/
│   ├── base.html       # Shared layout: top nav, collapsible file-tree sidebar
│   ├── directory.html  # Directory listing view
│   ├── file.html       # Code viewer + markdown renderer
│   └── plots.html      # PNG gallery with lightbox
└── static/
    └── style.css       # GitHub-palette design system
```

## Dependencies

```toml
# pixi.toml
flask = "*"
pygments = "*"
```

CDN libraries loaded by the browser (no install required):
- **marked.js** — GitHub Flavoured Markdown parsing
- **highlight.js** — syntax highlighting inside markdown code fences

## Running

`ROOT` can be supplied as a CLI argument, an environment variable, or defaults
to the current working directory.

```bash
# Browse a specific project
python web/app.py /path/to/project

# Override port (default 9000)
PORT=8888 python web/app.py /path/to/project

# Run in the background
nohup python web/app.py /path/to/project > /tmp/web.log 2>&1 &
echo $! > /tmp/web.pid

# Stop it
kill $(cat /tmp/web.pid)
```

## Accessing from the Mac

Inside OrbStack the container is reachable via its IP address, not `localhost`.

```bash
hostname -I   # get the container IP
```

Then open `http://<container-ip>:9000` in your Mac browser.

## Features

**File browser** — collapsible sidebar with directory tree; auto-expands and
highlights the current file on page load.

**Code viewer** — server-side syntax highlighting via Pygments (C++, Python,
YAML, CMake, SQL, TOML, and more). Line numbers are clickable anchor links
(`#L42`); a Copy button copies the raw source.

**Markdown rendering** — `.md` files open in Rendered view by default, parsed
by marked.js with GitHub Flavoured Markdown support (tables, task checkboxes,
fenced code blocks with syntax highlighting). A Rendered / Source toggle
switches between the two views.

**Plot gallery** — `/plots` scans the project tree for `.png` files and shows
a thumbnail grid. Click to open a full-size lightbox; press Escape to close.
