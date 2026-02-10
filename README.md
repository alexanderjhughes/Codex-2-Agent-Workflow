# Codex Two-Agent Loop

This directory contains scripts and documentation for running a two-agent loop using Codex CLI in non-interactive mode. The main script, `codex-loop.sh`, automates a worker/manager agent workflow for feature delivery, with strict context control and logging.

## Key Features

- Two-agent loop (worker/manager) using Codex CLI
- Small context window: worker re-reads spec + progress file each turn
- Only last worker/manager messages are passed between turns
- Creates a new branch for each feature
- Logs all output to a top-level log file
- Commits all changes with `feat:` or `fix:` (manager chooses)
- Moves completed specs to `specs/completed/`
- Appends summaries to `specs/completed/featuresCompletedLog.md`
- Creates and opens a PR with `gh` after all specs complete
- Uses `PULL_REQUEST_TEMPLATE.md` when present, otherwise generates a minimal PR body

## Usage

```bash
chmod +x codex-loop
./codex-loop my-feature-branch
```

## If you want, you can also add it to your PATH for easier access

Add this line to your shell configuration file (e.g., `~/.bashrc`, `~/.zshrc`):
```bash
export PATH="$HOME/.local/scripts:$PATH"
```

### PR Creation Behavior

After the loop finishes all specs, the script will:

- Create a PR using `gh pr create` with the branch name as the PR title
- Use `PULL_REQUEST_TEMPLATE.md` if it exists in the repo
- Fall back to a default PR body that includes the PR title and a note that it was created by the Codex loop CLI tool
- Open the PR in your browser with `gh pr view --web`

If PR creation fails for any reason (including missing `gh`), the script exits safely with a color-coded warning/error.

See the script for more details and required dependencies.
