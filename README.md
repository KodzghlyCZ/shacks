# shacks

Small shell helpers for an interactive session: **`pu`** (listening ports), **`gfp`** (`git fetch && git pull`, optional args go to **`git pull`** only), **`retry`** (re-run a command until it exits 0), and **`shacks-update`** (`git pull --ff-only` in **`SHACKS_REPO`**—this clone). You either set that directory once or let bash/zsh infer it when you source the snippet.

## Install

Clone the repo (use any directory you like instead of `~/src/shacks`; keep the path consistent with the lines you add to your shell config):

```bash
git clone https://github.com/KodzghlyCZ/shacks.git ~/src/shacks
```

Then add to **`~/.bashrc`** or **`~/.zshrc`** (adjust the path to match your clone):

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

Use an **interactive** shell so **`shacks-update`** (an alias) is expanded; **`pu`**, **`gfp`**, and **`retry`** are functions and follow the same expectation for a normal login session.

## Update

When you open a new shell and `shacks` is sourced, it starts a **background** **`git fetch`** so your prompt is not delayed; if your clone is behind its upstream, a warning is printed to **stderr** whenever that fetch finishes (often right after your prompt appears). That still needs network on those logins. To turn it off, **`export SHACKS_SKIP_UPDATE_CHECK=1`** before sourcing, or change the default at the **top of the `shacks` file** from `:-0` to `:-1` in your copy.

After loading `shacks`, apply updates with:

```bash
shacks-update
```

That uses the **`SHACKS_REPO`** from your session (see install above).

**Note:** If `shacks-update` fails or aliases are not available, pull from your clone manually with `git -C /path/to/shacks pull --ff-only` (use your real clone path instead of `/path/to/shacks`).

## Contributing

Ideas and improvements are welcome. If you are comfortable with git and shell snippets, open a **pull request** (or merge request, depending on where you host a fork) with your change and a short note on what it does and why.

If you do not write code or prefer not to touch the repo, that is fine too—open an **issue** instead. Describe the idea, pain point, or alias you wish you had; someone can pick it up or discuss it there.

## Requirements

- `pu`: `sudo`, `lsof`, `awk`.
- `gfp`: `git`. Runs in whatever directory you are in (not limited to **`SHACKS_REPO`**). If you use **Oh My Zsh** with the **`git`** plugin instead, there is no single **`gfp`** alias, but **`gfa`** (`git fetch --all`) plus **`gl`** (`git pull`) is close; see the [git plugin README](https://github.com/ohmyzsh/ohmyzsh/tree/master/plugins/git). Plain **`git pull`** already fetches the upstream for the current branch, so **`gfp`** is for when you want an explicit **`fetch`** first (same defaults as **`git fetch`** with no args).
- `retry`: `sleep` (and a shell with functions — bash or zsh is enough). Usage: `retry [-n max] [-s delay] command [args...]`. Omit **`-n`** (or use **`-n 0`**) for unlimited tries; **`-s`** defaults to **1** second between attempts. Use **`--`** if the command starts with `-`.
- `shacks-update`: `git` and a clone with `origin` (normal after `git clone`).
