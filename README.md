# fuzzbud

A collection of fzf-powered terminal tools. Install the ones you need.

## Install

### Quick install (user-local)

```bash
curl -fsSL https://raw.githubusercontent.com/oysteinje/fuzzbud/main/install.sh | bash
```

Installs `fuzzbud` to `~/.local/bin/fuzzbud`. Then pick your tools:

```bash
fuzzbud install
```

### System-wide install

```bash
curl -fsSL https://raw.githubusercontent.com/oysteinje/fuzzbud/main/install.sh | bash -s -- --system
```

Installs to `/usr/local/bin/fuzzbud`. Requires `sudo`.

### From a local clone

```bash
git clone https://github.com/oysteinje/fuzzbud.git
cd fuzzbud
./install.sh
```

## Usage

```
fuzzbud                      Interactive launcher — pick and run an installed tool
fuzzbud install              Pick tools to install interactively
fuzzbud install <tool>       Install a specific tool by name
fuzzbud install --tag TAG    Install all tools matching a tag
fuzzbud list                 Show all tools with install status
fuzzbud --init               Print shell integration (ctrl+r binding)
```

### Examples

```bash
# Install only Kubernetes-related tools
fuzzbud install --tag kubernetes

# Install only Azure-related tools
fuzzbud install --tag azure

# Install a single tool
fuzzbud install gha-fuzzy
```

### Shell integration

Add ctrl+r as a shortcut to the fuzzbud launcher:

```bash
eval "$(fuzzbud --init)"
```

Add to `~/.bashrc` or `~/.zshrc` to make it permanent.

## Available tools

| Tool | Tags | Description | Dependencies |
|---|---|---|---|
| [aks-connector](scripts/aks-connector/) | `azure` `kubernetes` | Browse and connect to AKS clusters across your Azure tenant | fzf az jq kubelogin kubectl |
| [kube-fuzzy](scripts/kube-fuzzy/) | `kubernetes` | Navigate Kubernetes resources and run actions interactively | fzf kubectl |
| [gha-fuzzy](scripts/gha-fuzzy/) | `github` | Browse and trigger GitHub Actions workflows and runs | fzf gh |
| [gh-pr-fuzzy](scripts/gh-pr-fuzzy/) | `github` | Browse pull requests and take common actions interactively | fzf gh |
| [pim-me-up](scripts/pim-me-up/) | `azure` | Activate Azure PIM roles from the terminal | fzf az jq curl |

## Install individual tools directly

Each tool has its own install script if you prefer not to go through `fuzzbud`:

```bash
# aks-connector
curl -fsSL https://raw.githubusercontent.com/oysteinje/fuzzbud/main/scripts/aks-connector/install.sh | bash

# kube-fuzzy
curl -fsSL https://raw.githubusercontent.com/oysteinje/fuzzbud/main/scripts/kube-fuzzy/install.sh | bash

# gha-fuzzy
curl -fsSL https://raw.githubusercontent.com/oysteinje/fuzzbud/main/scripts/gha-fuzzy/install.sh | bash

# gh-pr-fuzzy
curl -fsSL https://raw.githubusercontent.com/oysteinje/fuzzbud/main/scripts/gh-pr-fuzzy/install.sh | bash

# pim-me-up
curl -fsSL https://raw.githubusercontent.com/oysteinje/fuzzbud/main/scripts/pim-me-up/install.sh | bash
```

## Supported environments

- Linux
- macOS with a modern Bash installation

Windows is not currently supported.

## Contributing

To add a new tool:

1. Add a directory under `scripts/<tool-name>/` with the script and an `install.sh`
2. Add one line to `registry`:
   ```
   tool-name|Short description|space-separated-deps|comma-separated-tags
   ```
3. Update the embedded `REGISTRY` array in `fuzzbud` to match
4. Open a pull request

See an existing script (e.g. `scripts/kube-fuzzy/`) for the expected structure.

## License

MIT.
