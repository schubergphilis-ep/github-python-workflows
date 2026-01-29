# Reusable GitHub Workflows for Python projects

This repository provides reusable GitHub Actions workflows for Python-based projects. The workflows are designed to standardize project bootstrapping and ongoing synchronization using the Paleofuturistic Python template, ensuring consistent structure, tooling, and quality defaults across repositories.

While primarily intended for use within our organization, these workflows are generic — anyone can reuse them to bootstrap and maintain Python repositories using Cruft-based templates.

## Available Workflows

### 🧬 Template Sync Workflow (`.github/workflows/template-sync.yaml`)

This workflow bootstraps or updates a repository from the
[`paleofuturistic_python`](https://github.com/schubergphilis/paleofuturistic_python) template using Cruft.

The workflow automatically detects whether the repository is already managed by Cruft:

- **Bootstrap mode**
  - Runs when no `.cruft.json` file is present
  - Generates a fresh project from the template
  - Syncs the generated content into the repository
  - Creates a pull request to finalize the initial setup

- **Update mode**
  - Runs when `.cruft.json` exists
  - Applies upstream template changes using `cruft update`
  - Creates a pull request with the proposed updates

#### What the template provides

- 📁 Project structure and documentation scaffolding
- 📦 Dependency and environment management (via `uv`)
- 🧪 Testing, linting, formatting, and type checking
- 🔐 Quality and CI automation
- 🚀 Build, release, and distribution workflows

## How to Use

To use this workflow in a Python repository, add the following file to
`.github/workflows/template-sync.yaml`:

```yaml
name: Python Template Sync

on:
  push:
    paths:
      - '.github/workflows/template-sync.yaml'
  schedule:
    - cron: "0 5 * * 1" # Weekly Monday 06:00 Europe/Amsterdam (05:00 UTC in winter)
  workflow_dispatch:

permissions:
  contents: read
  pull-requests: write

jobs:
  sync:
    uses: schubergphilis-ep/github-python-workflows/.github/workflows/template-sync.yaml@main
    with:
      project_description: "Short description of the project"
      project_name: "Example Project"
      project_slug: "example-project"
    secrets: inherit
```

> [!NOTE]
> Change `main` to the latest release to use a versioned workflow, e.g. `v1`.

## Requirements

- The repository must allow GitHub Actions to create branches and pull requests.
- GITHUB_TOKEN must have write permissions:

```yaml
permissions:
  contents: read
  pull-requests: write
```

## License

This repository is licensed under the Apache 2.0 License.

## Contributions

Contributions are welcome! Please open a pull request with clear commit messages and relevant test cases.
