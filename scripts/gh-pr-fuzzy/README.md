# gh-pr-fuzzy

Fast GitHub pull request browser from your terminal using fzf.

Browse pull requests, inspect summary data, review checks and comments, open diffs, check out the branch, approve, merge, or open the PR in your browser.

## Features

- **Open PRs by default**: Shows only open pull requests — no noise from closed/merged history
- **Inline preview**: PR metadata, checks, labels, changed files, and description while browsing
- **Review & checks at a glance**: Review decision and CI status visible in the list; ready-to-merge PRs (PASS + APPROVED) are bold-green in the `#` column
- **Rich action menu**: Summary, diff, checks, comments — plus write-actions: compose a comment in `$EDITOR`, post an approval/request-changes/comment review, copy PR URL or branch to the clipboard, pick squash/merge/rebase strategy, checkout, or open in the browser
- **In-place filter toggle**: `Ctrl-F` opens a filter sub-prompt to cycle state, toggle "mine" / drafts / "needs my review", or change the search/author/limit without leaving the tool
- **Reload on demand**: `Ctrl-R` re-fetches the list to surface fresh CI / review data
- **Open in browser**: `Ctrl-O` from the list
- **Zero config**: Uses your existing `gh` CLI authentication and repository context

## Dependencies

- [fzf](https://github.com/junegunn/fzf) (0.30+ recommended)
- [GitHub CLI](https://cli.github.com/) (`gh`)
- `awk`, `cut`, `sed`, `tr`
- Bash 4+
- Optional: `wl-copy` / `xclip` / `xsel` / `pbcopy` for clipboard actions; `$EDITOR` (falls back to `vi`) for `comment` / `review` composition

## Supported environments

- Linux
- macOS with a modern Bash installation

## Runtime assumptions

- You are already authenticated with `gh auth login`
- You are running the script inside a git repository, or passing a `[repo]` argument (for example `owner/repo`)

## Install

### Via fuzzbud

```bash
fuzzbud install gh-pr-fuzzy
```

### Quick install (user-local)

```bash
curl -fsSL https://raw.githubusercontent.com/oysteinje/fuzzbud/main/scripts/gh-pr-fuzzy/install.sh | bash
```

Installs to `~/.local/bin/gh-pr-fuzzy`.

### System-wide install

```bash
curl -fsSL https://raw.githubusercontent.com/oysteinje/fuzzbud/main/scripts/gh-pr-fuzzy/install.sh | bash -s -- --system
```

Installs to `/usr/local/bin/gh-pr-fuzzy`. Requires `sudo`.

### From a local clone

```bash
git clone https://github.com/oysteinje/fuzzbud.git
cd fuzzbud
./scripts/gh-pr-fuzzy/install.sh
```

## Usage

```
Usage: gh-pr-fuzzy [options] [owner/repo]

Options:
  -s, --state STATE   Filter by state: open (default), closed, merged, all
  -a, --all           Shorthand for --state all
  -d, --draft         Include draft PRs (hidden by default)
  -m, --mine          Show only your PRs (shorthand for --author @me)
  -A, --author USER   Filter by author (GitHub username)
  -S, --search QUERY  Search PR titles
  -r, --needs-review  Show only PRs where your review is requested
  -l, --limit N       Max PRs to fetch (default: 100)
  -h, --help          Show this help
```

### Examples

```bash
gh-pr-fuzzy                          # Open PRs in current repo
gh-pr-fuzzy -a                       # All PRs (open, closed, merged)
gh-pr-fuzzy -m                       # Your open PRs
gh-pr-fuzzy -r                       # PRs awaiting your review
gh-pr-fuzzy -A octocat               # Open PRs by octocat
gh-pr-fuzzy -s merged owner/repo     # Merged PRs in a specific repo
gh-pr-fuzzy -S "fix login"           # Search open PRs by title
```

### Keybindings

In the PR list:

| Key       | Action                                                   |
|-----------|----------------------------------------------------------|
| Enter     | Select PR and open action menu                           |
| Ctrl-O    | Open selected PR in browser                              |
| Ctrl-R    | Reload PR list                                           |
| Ctrl-F    | Open filter sub-prompt (state / mine / drafts / review)  |
| Esc       | Exit                                                     |

### Actions

After selecting a PR:

- `summary` — View the main PR summary
- `diff` — View the PR diff
- `checks` — View GitHub check results
- `comments` — View PR comments and review discussion
- `comment` — Compose a comment in `$EDITOR` and post it (save empty to cancel)
- `review` — Post a review: pick `approve` / `request-changes` / `comment-only`; the latter two open `$EDITOR` for the body
- `checkout` — Check out the PR branch locally
- `merge` — Pick strategy (`squash` / `merge` / `rebase`), then confirm
- `copy-url` — Copy the PR URL to the clipboard (`wl-copy` / `xclip` / `xsel` / `pbcopy`)
- `copy-branch` — Copy the head branch name to the clipboard
- `browser` — Open the PR in your default browser

Press Esc to go back to the PR list.

### Filter sub-prompt (Ctrl-F)

One key opens a small menu listing every filter and its current value. Pick a row to change it:

- `state` cycles `open → merged → closed → all`
- `mine` toggles `--author @me` on / off
- `drafts` toggles draft visibility
- `review of me` toggles `review-requested:@me`
- `search` / `author` / `limit` — prompt for a new value (empty clears)

The PR list reloads in place after each change.

## License

MIT.
