# WENet Deploy Action

GitHub Action wrapper for the `wenet` CLI.

This action is intentionally thin:

1. Download the released `wenet` binary for the runner OS/architecture.
2. Export `API_TOKEN` and `API_SERVER`.
3. Run `wenet deploy` in the directory containing `edge.toml`.

All packaging and deployment behavior lives in the CLI, not in this action.

## Usage

```yaml
name: Deploy

on:
  push:
    branches:
      - main

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v6

      - name: Deploy to WENet
        uses: wenet-ec/edge-action@v1
        with:
          api-token: ${{ secrets.WENET_API_TOKEN }}
```

If your `edge.toml` is not at the repository root:

```yaml
- name: Deploy to WENet
  uses: wenet-ec/edge-action@v1
  with:
    api-token: ${{ secrets.WENET_API_TOKEN }}
    working-directory: ./apps/web
```

Pin a specific CLI version:

```yaml
- name: Deploy to WENet
  uses: wenet-ec/edge-action@v1
  with:
    api-token: ${{ secrets.WENET_API_TOKEN }}
    cli-version: v0.1.0
```

## Inputs

| Input | Required | Default | Description |
| --- | --- | --- | --- |
| `api-token` | yes |  | WENet public API token. |
| `api-server` | no | `https://api.wenet-ec.com` | WENet public API server. |
| `cli-version` | no | `latest` | `wenet` CLI release tag to install. |
| `cli-repository` | no | `wenet-ec/wenet-cli` | Repository that publishes CLI releases. |
| `working-directory` | no | `.` | Directory containing `edge.toml`. |

## edge.toml

The project being deployed must include `edge.toml`:

```toml
project = "my-web-server"
tag = "1.2.0"

[scripts]
linux = "deploy.sh"
darwin = "deploy.sh"
windows = "deploy.ps1"

all = true
```

See the `wenet-cli` repository for the full config format.
