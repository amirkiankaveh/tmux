# Repository Guidelines

## Project Structure & Module Organization
This tmux profile is anchored by the upstream bundle in `oh-my-tmux/`, while `tmux.conf` provides the tracked base configuration and `tmux.conf.local` holds our custom defaults. Community plugins live in `plugins/`: `tpm/` (manager), `tmux2k/` (status bar framework), `tmux-menus/`, and helpers such as `tmux-cht-sh/`. Statusline modules are individual Bash scripts under `plugins/tmux2k/plugins/*.sh`, with shared helpers in `plugins/tmux2k/lib/utils.sh`. Add new assets alongside screenshots in `plugins/tmux2k/images/`.

## Build, Test, and Development Commands
- `tmux source-file ~/.config/tmux/tmux.conf` — reloads the full configuration inside an attached session.
- `TMUX_CONF=~/.config/tmux/tmux.conf tmux -f ~/.config/tmux/tmux.conf new` — launches a throwaway session for local validation.
- `~/.config/tmux/plugins/tpm/bin/install_plugins` and `.../update_plugins` — sync TPM-managed dependencies.
- `~/.config/tmux/plugins/tpm/bin/clean_plugins` — prune unused plugins after updates.

## Coding Style & Naming Conventions
Write shell code in Bash 5 syntax, indent with four spaces, and prefer long-form flags for readability. Keep function names lowercase with hyphen separators (`render-window-list`) and constants uppercase. Source shared helpers instead of duplicating logic, and declare dependencies near the top of each script. Run `shellcheck` on touched scripts and apply `shfmt -i 4 -ci` before submitting. Tmux option keys should follow the `@tmux2k-<feature>` pattern, matching file names under `plugins/tmux2k/plugins/`.

## Testing Guidelines
Reload the config with `tmux source-file` after every change and watch the status line for regressions. For plugins, log sample output with `bash plugins/tmux2k/plugins/<name>.sh` to ensure they emit a single line. Validate tmux options via `tmux show -gv @tmux2k-theme` and confirm no unexpected errors in the tmux client log. When altering key bindings, test both new and legacy prefixes (`C-b`, `C-a`).

## Commit & Pull Request Guidelines
Use imperative, scope-first commit subjects (`Add ping plugin cache`) and keep bodies concise, flagging any manual steps or environment changes. Group cosmetic and functional changes separately so reviewers can diff them in isolation. Pull requests should summarize visible changes (status line screenshots, binding tables), link to any related issue or upstream plugin PR, and list verification steps executed. Mention macOS- or Linux-specific testing explicitly to help other contributors reproduce results.

## Configuration Tips
Document new environment variables in `tmux.conf.local` comments and mirror them in this guide. Store secrets in the shell environment, never in tracked files, and prefer referencing external CLI tools through `command -v` checks to guard against missing dependencies.
