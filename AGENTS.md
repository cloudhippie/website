# Website — Agent Guide

This document provides guidance for AI coding agents (GitHub Copilot, Codex,
Claude, etc.) working in this repository.

---

## Git Workflow — MANDATORY

- **Never commit changes** unless explicitly instructed to do so.
- **Never create a branch** unless explicitly instructed to do so.
- **Never open a pull request** unless explicitly instructed to do so.
- Leave all changes as unstaged working-tree modifications by default.

---

## Shell Environment

This repository uses [**mise**](https://mise.jdx.dev) (`mise.toml`) to provide
the required tooling.

Run this once after cloning or after tool version changes:

```bash
mise trust && mise install
```

If mise is not activated in your shell, prefix commands with `mise exec --`:

```bash
mise exec -- npm install --ci
mise exec -- hugo server
```

`mise.toml` provides:

- **Node.js 22**
- **Hugo Extended 0.164.0**

Do not assume system-installed `node`, `npm`, or `hugo` versions are correct.
Use the mise-managed versions.

---

## Project Overview

This repository contains the **Hugo** static site for **cloudhippie.de**.

The README still describes the site as hosted on **Netlify**, while the current
`general.yml` workflow builds the site with **Hugo Extended** and uploads the
resulting `public/` directory to an **S3-compatible object store** using
`s5cmd`. Frontend assets are managed with **npm** and loaded into Hugo through
module mounts defined in `config.yml`.

---

## Repository Layout

```text
assets/
  website.js           Site JavaScript entrypoint
  website.scss         Site stylesheet entrypoint

content/
  index.md             Homepage content with YAML front matter

layouts/
  404.html             Custom 404 page
  index.html           Homepage layout
  _default/
    single.html        Default single-page template
    taxonomy.html      Default taxonomy template
  partials/
    header.html        Shared HTML head / asset includes
    navbar.html        Shared site navigation
    footer.html        Shared footer

static/
  favicon.ico          Public static asset
  hipone.svg           Public static asset

config.yml             Hugo site configuration and module mounts
package.json           npm dependencies for frontend assets
.github/workflows/     CI, release, and automerge workflows
mise.toml              Local toolchain definition
README.md              Project overview and local development notes
```

---

## Development

```bash
mise trust && mise install
npm install --ci
hugo server
```

Serves the site on http://localhost:1313 with live reload.

---

## Build

Use the same build command as CI:

```bash
hugo --cleanDestinationDir --enableGitInfo --minify --gc
```

Run `npm install --ci` first so the mounted frontend dependencies from
`node_modules/` are available.

---

## CI / GitHub Actions

| Workflow | Trigger | Purpose |
|---|---|---|
| `general.yml` | push / PR to `master` | Installs Node/Hugo, builds the site, then uploads `public/` to S3-compatible object storage via `s5cmd`. |
| `release.yml` | `workflow_dispatch`, weekly schedule | Runs semantic-release and updates `CHANGELOG.md` plus `.github/RELEASE`. |
| `automerge.yml` | `workflow_dispatch`, PRs to `master` | Approves and enables automerge for Dependabot pull requests. |

---

## Git & Contribution Conventions

- Merge commits are disabled in repository settings.
- Prefer **squash merges**; rebase merges are also allowed by the ruleset.
- Commit messages should follow the **Conventional Commits** types listed in
  `.github/semantic.yml`.
- Releases are automated with **semantic-release** via `release.yml`.
- Security issues should be reported privately to
  `security@cloudhippie.de`.

---

## Key Patterns to Follow

1. **Use YAML front matter**: content pages follow Hugo front matter fields like
   `draft`, `title`, and `weight` (`content/index.md`).
2. **Keep shared markup in partials**: header, navigation, and footer are
   composed from `layouts/partials/` instead of duplicated inline.
3. **Put build-time assets under `assets/`**: SCSS and JavaScript belong in the
   Hugo asset pipeline, while static passthrough files belong in `static/`.
4. **Keep Hugo mounts in sync**: if new third-party frontend assets are needed,
   update `config.yml` module mounts so Hugo can resolve them from
   `node_modules/`.
5. **Match CI build flags**: production-oriented changes should continue to work
   with `--cleanDestinationDir --enableGitInfo --minify --gc`.

---

## Metadata Changes

When updating `AGENTS.md`, `GEMINI.md`, and `CLAUDE.md`, treat `AGENTS.md` as
the primary instructions file, and `GEMINI.md`/`CLAUDE.md` as copies that
simply reference it (`@AGENTS.md`).

These files are collectively called the "agent instruction files".
