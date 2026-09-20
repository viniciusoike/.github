# .github

Shared reusable GitHub Actions workflows for my R packages.

## Available workflows

### `pkgdown.yml`

Builds and deploys a pkgdown site (derived from the
[r-lib actions examples](https://github.com/r-lib/actions/tree/v2/examples))
with one fix built in: **it removes internal agent docs** (`AGENTS.md`,
`CLAUDE.md`, … by default) before building, because pkgdown renders every
root-level `.md` file into the published site, sitemap and search index
(pkgdown is aware of this as
[r-lib/pkgdown#2959](https://github.com/r-lib/pkgdown/issues/2959)).

## Usage

Replace the package's `.github/workflows/pkgdown.yaml` with:

```yaml
on:
  push:
    branches: [main, master]
  pull_request:
    branches: [main, master]
  release:
    types: [published]
  workflow_dispatch:

name: pkgdown

permissions: read-all

jobs:
  pkgdown:
    uses: viniciusoike/.github/.github/workflows/pkgdown.yml@main
    with:
      internal_md_docs: "CLAUDE.md AGENTS.md"   # optional; this is the default
```

The triggering events must stay in the calling workflow — `workflow_call` can
only be armed once and reusable workflows cannot define `on.push` triggers
themselves.

The `github-pages` environment must allow the repo's default branch to deploy
(Settings → Environments → github-pages), same as any pkgdown Pages setup.
