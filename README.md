# blog

Small, stdlib-first Go server powering my technical blog and worklog —
long-form GPU/inference performance writeups and short progress notes.

**Status:** building v1 in the open. The commit history is the progress bar.

## Why hand-built

Two reasons this exists instead of a static-site generator:

1. **It's my Go re-entry project — the server is the curriculum.** All code in
   this repository is written by hand: no AI code generation, no Claude Code,
   no Copilot. The practice is the point, and I want to be able to defend
   every line.
2. **The posts are full of CUDA code and benchmark tables**, so I want the
   rendering pipeline under my own control: server-side syntax highlighting,
   real tables, no client-side JavaScript.

## Design

- **Stdlib first.** `net/http` with Go 1.22+ ServeMux patterns, `html/template`,
  `embed`, `log/slog`, `flag`. No framework, no router dependency, no database.
- **Git is the CMS.** Posts are markdown files under `content/posts/`.
  Publishing = commit → `make deploy` → systemd restart. All posts parse into
  memory at startup; there is nothing to administer.
- **Four third-party dependencies, total:**

| Package | Job |
|---|---|
| `yuin/goldmark` | markdown → HTML (CommonMark) |
| `yuin/goldmark-highlighting` (chroma) | server-side syntax highlighting, incl. CUDA/C++ |
| `gopkg.in/yaml.v3` | front-matter parsing |
| `gorilla/feeds` | RSS/Atom feed |

## Layout

```
main.go            entry point: flags, wiring
internal/post/     front matter, markdown → HTML, slugs, sorting
internal/server/   handlers, routes, middleware, feed
content/posts/     the posts (YYYY-MM-DD-slug.md) and their images
templates/         base / index / post / 404 (embedded)
static/            stylesheet, favicon (embedded)
deploy/            systemd unit, Caddyfile, server rebuild runbook
```

## Content format

One post = one markdown file with YAML front matter:

```
title:    string
date:     YYYY-MM-DD
slug:     string
kind:     writeup | note      # long-form artifact vs. short progress entry
draft:    bool                # hidden in production
summary:  one sentence        # index listing + meta/OG tags
```

## Run locally

```
make run            # serves on 127.0.0.1:8080
make run-drafts     # same, drafts visible
```

## Deploy

Single static binary (templates and static assets embedded) plus the
`content/` directory, rsynced to a VPS; systemd supervises the process and
Caddy terminates TLS in front of `127.0.0.1:8080`. Rebuilding the server from
a blank machine is documented in [`deploy/SETUP.md`](deploy/SETUP.md).

## License

Code: MIT. Post content under `content/`: © Bryce Fitzgerald, all rights
reserved.
