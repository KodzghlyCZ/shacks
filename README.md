# shacks

Small shell helpers for an interactive session: a `pu` alias (listening ports and owning processes) and **`shacks-update`**, which runs `git pull --ff-only` in **`SHACKS_REPO`** (this clone). You either set that directory once or let bash/zsh infer it when you source the snippet.

## Clone

```bash
git clone git@github.com:KodzghlyCZ/shacks.git ~/src/shacks
```

Use any directory you like instead of `~/src/shacks`.

## Install — `SHACKS_REPO` + one source line

Add to **`~/.bashrc`** or **`~/.zshrc`** (adjust the path):

```bash
SHACKS_REPO=$HOME/src/shacks
. "$SHACKS_REPO/shacks"
```

`$HOME/...` avoids surprises with `~` inside variables.

**Or** skip the variable and source the file by path; **bash** and **zsh** will set **`SHACKS_REPO`** to this repo’s root automatically:

```bash
. $HOME/src/shacks/shacks
```

Minimal POSIX shells that do not expose the sourced file’s path should use the two-line block with **`SHACKS_REPO`** set first.

Reload your shell (`exec bash -l`, `exec zsh -l`, or a new terminal) so this runs once per session.

Aliases need an **interactive** shell (bash and zsh enable aliases that way). Do not rely on a non-interactive `sh -c` session for `pu` / `shacks-update`.

## Update

When you open a new shell and `shacks` is sourced, it runs a quiet **`git fetch`** and prints a warning to **stderr** if your clone is behind its upstream (so you can run **`shacks-update`**). That needs network on those logins. To turn it off, **`export SHACKS_SKIP_UPDATE_CHECK=1`** before sourcing, or change the default at the **top of the `shacks` file** from `:-0` to `:-1` in your copy.

After loading `shacks`, apply updates with:

```bash
shacks-update
```

That uses the **`SHACKS_REPO`** from your session (see install above).

**Note:** If `shacks-update` fails or aliases are not available, pull from your clone manually with `git -C /path/to/shacks pull --ff-only` (use your real clone path instead of `/path/to/shacks`).

## Requirements

- `pu`: `sudo`, `lsof`, `awk`.
- `shacks-update`: `git` and a clone with `origin` (normal after `git clone`).
