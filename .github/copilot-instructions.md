# Copilot Instructions for SBOM Dashboard

## Project Overview

This repository is a GitHub Pages web dashboard that fetches **SPDX 2.3** SBOM (Software Bill of Materials) files from two source repositories and displays a unified, searchable dependency view. There is no backend — all data is fetched client-side from `raw.githubusercontent.com`.

**Live dashboard:** https://beardofedu.github.io/sbom-dashboard

## Repository Structure

- `index.html` — the entire application (HTML, Tailwind CSS via CDN, and vanilla JavaScript in a single file)
- `README.md` — project documentation

## Source Repositories

The dashboard reads SBOM data from these two repos:

| Repo | Ecosystem | SBOM path |
|------|-----------|-----------|
| [beardofedu/sbom-example-node](https://github.com/beardofedu/sbom-example-node) | Node.js / npm | `sbom/sbom.spdx.json` |
| [beardofedu/sbom-example-python](https://github.com/beardofedu/sbom-example-python) | Python / PyPI | `sbom/sbom.spdx.json` |

Each source repo uses a GitHub Actions workflow to call the [GitHub Dependency Graph SBOM API](https://docs.github.com/en/rest/dependency-graph/sboms) and commit the resulting `sbom/sbom.spdx.json` back to the repo on every push.

## Architecture

- The `REPOS` array at the top of the `<script>` block in `index.html` defines which repos are loaded. Each entry has a `key`, `name`, `owner`, `url` (raw SBOM URL), `color`, and `ecosystem`.
- `fetchSbom(repo)` fetches the SBOM JSON for a given repo.
- `extractPackages(sbom)` filters out the root repo package (identified by a `pkg:github/` purl) and returns the dependency list.
- Packages are de-duplicated across repos by `name@version` and merged into a unified `allPackages` array.
- The UI renders a summary card strip, a searchable/filterable package table, a side-by-side repo comparison list, and a license breakdown section.

## Adding a New Source Repo

Edit the `REPOS` array in `index.html`:

```js
const REPOS = [
  // existing entries …
  {
    key: 'myrepo',
    name: 'my-new-repo',
    owner: 'beardofedu',
    url: 'https://raw.githubusercontent.com/beardofedu/my-new-repo/main/sbom/sbom.spdx.json',
    color: 'purple',
    ecosystem: 'cargo',
  },
];
```

Also add a matching `<option>` to the `#filter-repo` `<select>` and a side-list `<div>` in the comparison section.

## Coding Conventions

- **Single-file app** — keep all HTML, CSS, and JavaScript in `index.html`.
- **Tailwind CSS via CDN** — use utility classes; avoid writing custom CSS except for the small `<style>` block already present.
- **Vanilla JS** — no frameworks or build tools; keep the code readable and dependency-free.
- **SPDX 2.3** — the dashboard expects the `packages` array from the SPDX JSON format produced by the GitHub Dependency Graph API.
