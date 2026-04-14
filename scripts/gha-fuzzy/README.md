# gha-fuzzy

Fast GitHub Actions workflow browser from your terminal using fzf.

Browse workflows, view runs, check job logs, and open runs in the browser — all with fuzzy search.

## Features

- **Workflow selection**: Instantly filter and select your GitHub Actions workflows
- **Run browsing**: Select specific workflow runs with color-coded status indicators
- **Action menu**: Choose between summary, logs (specific job or all jobs), and browser view
- **Formatted logs**: Cleans up raw `gh` log output and injects readable step headers
- **Zero config**: Uses your existing `gh` CLI authentication and repository context

## Dependencies

- [fzf](https://github.com/junegunn/fzf)
- [GitHub CLI](https://cli.github.com/) (`gh`)
- `awk`
- `cut`
- Bash 4+

## Supported environments

- Linux
- macOS with a modern Bash installation

## Runtime assumptions

- You are already authenticated with `gh auth login`
- You are running the script inside a git repository, or passing a `[repo]` argument (e.g. `owner/repo`)

## Install

### Via fuzzbud

```bash
fuzzbud install gha-fuzzy
```

### Quick install (user-local)

```bash
curl -fsSL https://raw.githubusercontent.com/oysteinje/fuzzbud/main/scripts/gha-fuzzy/install.sh | bash
```

Installs to `~/.local/bin/gha-fuzzy`.

### System-wide install

```bash
curl -fsSL https://raw.githubusercontent.com/oysteinje/fuzzbud/main/scripts/gha-fuzzy/install.sh | bash -s -- --system
```

Installs to `/usr/local/bin/gha-fuzzy`. Requires `sudo`.

### From a local clone

```bash
git clone https://github.com/oysteinje/fuzzbud.git
cd fuzzbud
./scripts/gha-fuzzy/install.sh
```

## Usage

```bash
# Make sure you're logged in with gh first
gh auth login

# Run in the current repository
gha-fuzzy

# Or run against a specific repository
gha-fuzzy owner/repo
```

1. Select a workflow (type to fuzzy search, Esc to quit)
2. Select a specific run (color-coded by success/failure)
3. Choose an action:
   - `summary`: View a quick summary of the run
   - `logs`: Select a specific job to view its formatted logs in `less`
   - `logs (all)`: View formatted logs for all jobs in the run
   - `browser`: Open the run in your default web browser
4. Press Esc to go back up a level at any time

## How it works

- **Workflows**: Queries the GitHub API via `gh workflow list` for available workflows
- **Runs**: Fetches recent runs for the selected workflow using `gh run list`
- **Logs**: Downloads logs via `gh run view --log`, formats them using `awk` to inject step headers from the GitHub API, and pipes the output to `less` for easy reading

## License

MIT.
