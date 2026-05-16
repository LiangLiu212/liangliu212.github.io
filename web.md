# Project Web Viewer

A self-contained Flask application that turns any project directory into a
browsable web interface with syntax-highlighted source code, rendered markdown
documents, and a plot gallery. Designed to run inside a development container
and be accessed from a Mac browser via the container IP.

---

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

---

## Dependencies

Add to your project's `pixi.toml`:

```toml
flask = "*"
pygments = "*"
```

Or install directly:

```bash
pip install flask pygments
```

CDN libraries (loaded by the browser, no install required):

- **marked.js** — GitHub Flavoured Markdown parsing
- **highlight.js** — syntax highlighting inside markdown code fences

---

## Running

`ROOT` can be supplied as a CLI argument, an environment variable, or defaults
to the current working directory.

```bash
# Browse a specific project
python web/app.py /path/to/project

# Browse the current directory
cd /path/to/project
python web/app.py

# Override port (default 9000)
PORT=8888 python web/app.py /path/to/project
```

Run in the background and keep it alive across terminal sessions:

```bash
nohup python web/app.py /path/to/project > /tmp/web.log 2>&1 &
echo $! > /tmp/web.pid
```

Stop it:

```bash
kill $(cat /tmp/web.pid)
```

---

## Accessing from the Mac

Inside OrbStack the container is reachable via its IP address, **not**
`localhost`. Get the IP with:

```bash
hostname -I
```

Then open `http://<container-ip>:9000` in your Mac browser.

> `localhost` will refuse the connection unless the container was started with
> `-p 9000:9000`. Using the container IP directly always works.

---

## Features

### File browser

A collapsible sidebar shows the full directory tree. Clicking a folder
navigates into it; clicking a file opens the viewer. The sidebar auto-expands
and highlights the current file on page load.

Directories and binary files (`.sqlite`, `.o`, `.so`, …) larger than 2 MB are
handled gracefully — binary files show a download link instead of attempting
to render content.

### Code viewer

Source files are syntax-highlighted server-side by Pygments. Supported
languages include C++, Python, YAML, CMake, SQL, TOML, Markdown, and more —
detected automatically from the file extension.

Line numbers are clickable anchor links: navigating to `#L42` scrolls to and
highlights that line. A **Copy** button copies the raw source text.

### Markdown rendering

`.md` files open in **Rendered** view by default. The content is parsed by
marked.js in the browser and styled to match GitHub's markdown appearance:

- Tables, task-list checkboxes, strikethrough, autolinks
- Fenced code blocks with syntax highlighting
- Blockquotes, images, horizontal rules

A **Rendered / Source** toggle in the panel header switches between the
formatted view and the raw highlighted source.

### Plot gallery

The `/plots` route scans the entire project tree for `.png` files and displays
them as a thumbnail grid. Click any thumbnail to open a full-size lightbox;
press `Escape` to close.

