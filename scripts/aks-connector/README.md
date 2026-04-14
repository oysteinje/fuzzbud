# aks-connector

Fast AKS cluster connection from your terminal using fzf.

Lists all Kubernetes clusters across every subscription in your tenant and connects to the one you pick.

## Features

- **Cross-subscription discovery**: Finds all AKS clusters in your tenant via Azure Resource Graph
- **Fuzzy search**: Find clusters instantly with fzf
- **Single-select**: Pick the cluster you want to connect to
- **Zero config**: Uses your existing `az login` session

## Dependencies

- [fzf](https://github.com/junegunn/fzf)
- [jq](https://jqlang.github.io/jq/)
- [Azure CLI](https://learn.microsoft.com/en-us/cli/azure/install-azure-cli) (`az`)
- [kubelogin](https://github.com/Azure/kubelogin)
- [kubectl](https://kubernetes.io/docs/tasks/tools/)

## Supported environments

- Linux
- macOS with a modern Bash installation

Windows is not currently supported.

## Runtime assumptions

- You are already authenticated with `az login`
- Azure CLI has a readable local profile at `~/.azure/azureProfile.json`
- `aks-connector` uses your existing Azure CLI session and does not store credentials

## Install

### Via fuzzbud

```bash
fuzzbud install aks-connector
```

### Quick install (user-local)

```bash
curl -fsSL https://raw.githubusercontent.com/oysteinje/fuzzbud/main/scripts/aks-connector/install.sh | bash
```

Installs to `~/.local/bin/aks-connector`.

### System-wide install

```bash
curl -fsSL https://raw.githubusercontent.com/oysteinje/fuzzbud/main/scripts/aks-connector/install.sh | bash -s -- --system
```

Installs to `/usr/local/bin/aks-connector`. Requires `sudo`.

### From a local clone

```bash
git clone https://github.com/oysteinje/fuzzbud.git
cd fuzzbud
./scripts/aks-connector/install.sh
```

## Usage

```bash
# Make sure you're logged in first
az login

# Run it
aks-connector
```

1. The tool fetches all AKS clusters in your tenant
2. Search and select a cluster with fzf
3. Done — kubectl is now configured and authenticated

## How it works

- Queries Azure Resource Graph for all `microsoft.containerservice/managedclusters` resources across all subscriptions in your tenant — one API call regardless of how many subscriptions you have
- Runs `az aks get-credentials` to merge the cluster credentials into your kubeconfig
- Runs `kubelogin convert-kubeconfig -l azurecli` so authentication piggybacks on your Azure CLI session

No extra credentials or service principals needed.

## License

MIT.
