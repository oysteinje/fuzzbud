# docker-fuzzy

Interactive Docker browser for your terminal using fzf.

Navigate containers, images, volumes, and networks without memorizing `docker` commands or flags. No keybindings to learn — everything is a list you navigate and confirm with enter.

## Features

- **Menu-driven browsing**: Pick resource type, resource, and action — all through fzf
- **Full resource coverage**: Containers, Images, Volumes, Networks
- **Inline preview**: Recent logs for containers, layer history for images, inspect output for volumes and networks
- **Color-coded container status**: Running containers in green, stopped in red
- **Context-aware actions**: Each resource type gets relevant actions (logs, exec, inspect, start, stop, remove, etc.)
- **Prune workflows**: Prune stopped containers, dangling images, unused volumes, networks, or everything at once
- **Zero config**: Uses your existing Docker daemon

## Dependencies

- [fzf](https://github.com/junegunn/fzf)
- [docker](https://docs.docker.com/engine/install/)

## Supported environments

- Linux
- macOS with a modern Bash installation

Windows is not currently supported.

## Runtime assumptions

- Docker daemon is running and accessible (`docker info` succeeds)
- `docker-fuzzy` uses your existing Docker context and does not store credentials

## Install

### Via fuzzbud

```bash
fuzzbud install docker-fuzzy
```

### Quick install (user-local)

```bash
curl -fsSL https://raw.githubusercontent.com/oysteinje/fuzzbud/main/scripts/docker-fuzzy/install.sh | bash
```

Installs to `~/.local/bin/docker-fuzzy`.

### System-wide install

```bash
curl -fsSL https://raw.githubusercontent.com/oysteinje/fuzzbud/main/scripts/docker-fuzzy/install.sh | bash -s -- --system
```

Installs to `/usr/local/bin/docker-fuzzy`. Requires `sudo`.

### From a local clone

```bash
git clone https://github.com/oysteinje/fuzzbud.git
cd fuzzbud
./scripts/docker-fuzzy/install.sh
```

## Usage

```bash
docker-fuzzy
```

1. The tool shows your Docker daemon version
2. Pick a resource type (Containers, Images, Volumes, Networks, or Prune...)
3. Pick a resource (with preview in the fzf pane)
4. Pick an action — done

Press Esc at any level to go back. The tool loops until you exit the resource type picker.

## Available actions

| Resource type | Actions |
|---|---|
| Containers | View logs, Follow logs, Exec into shell, Inspect, Start, Stop, Restart, Remove |
| Images | Inspect, History, Tag, Remove |
| Volumes | Inspect, Remove |
| Networks | Inspect, Remove |
| Prune... | Containers (stopped), Images (dangling), Volumes (unused), Networks (unused), System (all) |

## How it works

- Containers are listed with `docker ps -a`, color-coded by status (green = running, red = stopped/exited)
- The fzf preview pane shows recent container logs, image layer history, or inspect output depending on the resource type
- Inspect output is formatted with `jq` if available, otherwise `python3 -m json.tool`
- Destructive actions (Remove, Prune) always prompt for confirmation before executing

## License

MIT.
