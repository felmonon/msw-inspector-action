# msw-inspector-action

GitHub Action wrapper for `msw-inspector` coverage reports.

This repository exists so the Action can live in its own Marketplace-friendly repository. The analyzer itself stays in the main CLI repository:

- CLI repo: https://github.com/felmonon/msw-inspector
- npm package: `msw-inspector-cli`

## Usage

Generate a JSON report with the CLI, then feed that file to the Action:

```yaml
name: MSW coverage

on:
  pull_request:

jobs:
  inspect:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: 20
          cache: npm
      - run: npm ci
      - run: npx msw-inspector --report-file msw-inspector.json --format json
      - uses: felmonon/msw-inspector-action@v1
        with:
          summary-file: msw-inspector.json
          comment: true
```

## What It Does

- writes a compact job summary to `GITHUB_STEP_SUMMARY`
- exposes coverage and count outputs for later workflow steps
- can create or update a sticky PR comment with unmocked calls and stale handlers

## Repository Shape

This repository intentionally stays small:

- `action.yml`
- `dist/index.js`
- `README.md`
- `LICENSE`
