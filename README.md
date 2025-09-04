# jbro-test

GitHub Action to run the Microsoft Defender CLI (linux x64).

This repository contains the `jbro-test` action. Publish a release tag `v1` to provide a stable Marketplace entry; later you can add `v2` as a new tag/branch to publish a major revision.

Usage
-----

Create a workflow that uses the action:

```yaml
name: Defender CLI scan
on: [push]

jobs:
  scan:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Run jbro-test (Defender CLI)
        uses: jbrotsos/jbro-test-ghaction@v1
        with:
          args: 'scan --path .'
```

Notes
-----
- The action currently only supports Linux runners because the CLI is a linux-x64 binary.
- To publish to the Marketplace, create a public repository named `jbro-test`, push this code, and create a release tag `v1` (for example `git tag v1 && git push origin v1`).
- When you're ready to release a breaking change, create a `v2` tag and update the Marketplace listing accordingly.

Secrets / environment variables
------------------------------
This action requires three environment variables to authenticate the Defender CLI:

- `GDN_MDC_CLI_TENANT_ID`
- `GDN_MDC_CLI_CLIENT_ID`
- `GDN_MDC_CLI_CLIENT_SECRET`

Set these as GitHub Secrets in your repository (Settings → Secrets → Actions) and expose them to the workflow like this:

```yaml
steps:
  - uses: actions/checkout@v4
  - name: Run jbro-test
    uses: <owner>/jbro-test@v1
    env:
      GDN_MDC_CLI_TENANT_ID: ${{ secrets.GDN_MDC_CLI_TENANT_ID }}
      GDN_MDC_CLI_CLIENT_ID: ${{ secrets.GDN_MDC_CLI_CLIENT_ID }}
      GDN_MDC_CLI_CLIENT_SECRET: ${{ secrets.GDN_MDC_CLI_CLIENT_SECRET }}
    with:
      args: 'scan --path .'
```

Inputs
------
- `args` (optional): Command-line arguments to pass to the CLI. Defaults to `--help`.
- `cli-cache-dir` (optional): Path to cache the downloaded CLI.
