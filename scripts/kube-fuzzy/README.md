# kube-fuzzy

Interactive Kubernetes browser for your terminal using fzf.

Navigate namespaces, resources, and actions without memorizing kubectl commands or flags. No keybindings to learn — everything is a list you navigate and confirm with enter.

## Features

- **Menu-driven browsing**: Pick namespace, resource type, resource, and action — all through fzf
- **Wide resource coverage**: Pods, Deployments, StatefulSets, DaemonSets, Services, Ingresses, ConfigMaps, Secrets, PVCs, ServiceAccounts, Jobs, CronJobs, Nodes, Events
- **Browse any resource type**: The "Other..." option lists all namespaced API resources in your cluster
- **Inline preview**: See `kubectl describe` output in the fzf preview pane while selecting resources
- **Context-aware actions**: Each resource type gets relevant actions (logs, exec, port-forward, scale, restart, delete, etc.)
- **Zero config**: Uses your existing kubeconfig context

## Dependencies

- [fzf](https://github.com/junegunn/fzf)
- [kubectl](https://kubernetes.io/docs/tasks/tools/)

## Supported environments

- Linux
- macOS with a modern Bash installation

Windows is not currently supported.

## Runtime assumptions

- You have an active kubeconfig context (`kubectl config current-context` succeeds)
- `kube-fuzzy` uses your existing kubeconfig and does not store credentials

## Install

### Via fuzzbud

```bash
fuzzbud install kube-fuzzy
```

### Quick install (user-local)

```bash
curl -fsSL https://raw.githubusercontent.com/oysteinje/fuzzbud/main/scripts/kube-fuzzy/install.sh | bash
```

Installs to `~/.local/bin/kube-fuzzy`.

### System-wide install

```bash
curl -fsSL https://raw.githubusercontent.com/oysteinje/fuzzbud/main/scripts/kube-fuzzy/install.sh | bash -s -- --system
```

Installs to `/usr/local/bin/kube-fuzzy`. Requires `sudo`.

### From a local clone

```bash
git clone https://github.com/oysteinje/fuzzbud.git
cd fuzzbud
./scripts/kube-fuzzy/install.sh
```

## Usage

```bash
kube-fuzzy
```

1. The tool shows your current cluster and context
2. Pick a namespace
3. Pick a resource type
4. Pick a resource (with describe preview)
5. Pick an action — done

Press Esc at any level to go back. The tool loops until you exit the namespace picker.

## Available actions

| Resource type | Actions |
|---|---|
| Pods | View logs, Follow logs, Exec into shell, Describe, Delete |
| Deployments | Describe, Restart, Scale, Delete |
| StatefulSets | Describe, Restart, Scale, Delete |
| DaemonSets | Describe, Restart, Delete |
| Services | Port-forward, Describe, Delete |
| Ingresses | Describe, Delete |
| ConfigMaps | Describe, Delete |
| Secrets | View decoded, Describe, Delete |
| PVCs | Describe, Delete |
| ServiceAccounts | Describe, Delete |
| Jobs | View logs, Describe, Delete |
| CronJobs | Describe, Delete |
| Nodes | Describe |
| Events | Browse events sorted by timestamp |

## License

MIT.
