# SBOM Dashboard

A GitHub Pages web dashboard that reads **SPDX 2.3** SBOM files from two example repositories and displays a unified dependency view.

🔗 **Live dashboard:** https://beardofedu.github.io/sbom-dashboard

## Features

- 📦 Unified package table across all repos
- 🔍 Search and filter by repo, license, or package name
- 📊 License breakdown with visual bars
- 🆚 Side-by-side repo comparison
- ⚡ No backend — reads directly from `raw.githubusercontent.com`

## Source repos

| Repo | Ecosystem | SBOM |
|------|-----------|------|
| [sbom-example-node](https://github.com/beardofedu/sbom-example-node) | Node.js / npm | [sbom.spdx.json](https://github.com/beardofedu/sbom-example-node/blob/main/sbom/sbom.spdx.json) |
| [sbom-example-python](https://github.com/beardofedu/sbom-example-python) | Python / PyPI | [sbom.spdx.json](https://github.com/beardofedu/sbom-example-python/blob/main/sbom/sbom.spdx.json) |

## How it works

1. Each example repo has a GitHub Actions workflow that calls the [GitHub Dependency Graph SBOM API](https://docs.github.com/en/rest/dependency-graph/sboms)
2. The resulting `sbom/sbom.spdx.json` is committed back to each repo on every push
3. This dashboard fetches those files from `raw.githubusercontent.com` and renders the comparison

## Adding more repos

To add another repo to the dashboard, edit the `REPOS` array at the top of the `<script>` block in `index.html`:

```js
const REPOS = [
  // existing repos...
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

## Learn more

- [GitHub SBOM documentation](https://docs.github.com/en/code-security/supply-chain-security/understanding-your-software-supply-chain/exporting-a-software-bill-of-materials-for-your-repository)
- [SPDX specification](https://spdx.dev/specifications/)
