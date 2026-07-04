# AGENTS.md

This repository is a QMK userspace repo. Prefer the repo's existing QMK
userspace flow and keep changes scoped to keymaps, `qmk.json`, and related QMK
configuration unless the user asks otherwise.

## Environment

- The devcontainer image is `ghcr.io/qmk/qmk_cli`.
- `.devcontainer/setup.sh` configures QMK inside the container:
  - `user.qmk_home=/workspaces/qmk_firmware`
  - `user.overlay_dir=/workspaces/qmk_userspace`
- The host `qmk` CLI may not expose `compile`. For builds and QMK commands that
  require the full CLI, run them through the devcontainer.

Useful checks:

```sh
devcontainer exec --workspace-folder . qmk config -ro user.qmk_home
devcontainer exec --workspace-folder . qmk config -ro user.overlay_dir
```

## Current Keymap

The active Planck keymap lives at:

```text
keyboards/planck/rev6_drop/keymaps/planck_keymap/
```

`qmk.json` registers this build target:

```text
planck/rev6_drop:planck_keymap
```

## Common Commands

Create a fresh keymap:

```sh
devcontainer exec --workspace-folder . qmk new-keymap -kb planck/rev6_drop -km planck_keymap
```

Add it to userspace build targets:

```sh
devcontainer exec --workspace-folder . qmk userspace-add -kb planck/rev6_drop -km planck_keymap
```

Compile the keymap:

```sh
devcontainer exec --workspace-folder . qmk compile -kb planck/rev6_drop -km planck_keymap
```

Compile all userspace targets:

```sh
devcontainer exec --workspace-folder . qmk userspace-compile
```

Successful Planck compile outputs include:

```text
.build/planck_rev6_drop_planck_keymap.bin
.build/planck_rev6_drop_planck_keymap.hex
planck_rev6_drop_planck_keymap.bin
```

## Git Workflow Notes

- Keep generated firmware artifacts out of commits unless the user explicitly
  asks to commit them.
- Use focused branches for changes, for example `codex/add-planck-keymap`.
- For PRs, this repo is `toshikimiyagawa/qmk_userspace` with base branch `main`.
- A previous PR flow used a draft PR, then marked it ready before merge.
