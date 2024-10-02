# GitHub Action to Install and Setup UDS

Make [UDS CLI](https://github.com/defenseunicorns/uds-cli) available to your GitHub Actions workflows.

## Usage

```yaml
uses: defenseunicorns/setup-uds@v0.0.1
with:
  version: <version> # Optional. Defaults to latest.
```

## Example Run Task

```yaml
jobs:
  create_pacakge:
    runs-on: ubuntu-latest

    name: Run my cool UDS Task
    steps:
      - uses: actions/checkout@v3
        with:
          fetch-depth: 1

      - name: Install UDS CLI
        uses: defenseunicorns/setup-uds@v0.0.1
        with:
          version: v0.16.0

      - name: Run a UDS Task
        run: uds run my-task
```

## Example: bundle create, bundle deploy

```yaml
jobs:
  deploy_package:
    runs-on: ubuntu-latest

    name: Create & deploy my cool UDS Bundle
    steps:
      - uses: actions/checkout@v3
        with:
          fetch-depth: 1

      - name: Install Zarf
        uses: defenseunicorns/setup-uds@v0.0.1
        with:
          version: v0.16.0

      - name: Create the bundle
        run: uds create --confirm

      - name: Create a k3d cluster
        run: |
          curl -s https://raw.githubusercontent.com/k3d-io/k3d/main/install.sh | bash
          k3d cluster delete && k3d cluster create

      - name: Initialize the cluster
        run: uds deploy . --confirm

```

#

## Inputs

### version

- Optional
- Default: latest release
- **_Note:_** Include the `v` in your version (e.g., `v0.16.0`)
- Check out the [UDS releases page](https://github.com/defenseunicorns/uds-cli/releases) to see available versions
